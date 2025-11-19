
以下是我的源码内容，sign参数加密为MD5加密方式

```python
import requests,fake_useragent,time,re,execjs  
from hashlib import md5  
  
class TaoBao(object):  
  
    # 初始化数据，定义用户输入  
    def __init__(self):  
        self.basic_url = "https://h5api.m.taobao.com/h5/mtop.relationrecommend.wirelessrecommend.recommend/2.0/"  
        self.headers = {  
            "Referer":"https://s.taobao.com/",  
            'User-Agent': fake_useragent.UserAgent().random,  
            'cookie':"thw=xx; cna=wN4DI..."  
        }  
        # self.params = input("请输入你要获取的商品：")  
  
    # 获取加密参数sign  
    def get_sign(self,t,data):  
        """  
        生成加密参数sign  
        sign的组成：eE(em.token+"&"+eT+"&"+eC+"&"+ep.data)  
        :return: 加密后的sign值
        """
        # 从cookie中取出token  
        token = re.findall("_m_h5_tk=(.*?)_",self.headers['cookie'])[0]  
        eC = "12574478"  
        eT = t  
        sign_str = token + "&" + eT + "&" + eC + "&" + data  
  
        # 方法一：不知道加密方式时直接使用原始加密方式  
        with open("get_sign.js",'r') as f:  
            ctx = execjs.compile(f.read())  
        sign = ctx.call("eE",sign_str)  
  
        # 方法二：此处根据加密长度判断为MD5加密，因此可以使用第三方库进行加密  
        # sign = md5(sign_str.encode()).hexdigest()  
  
        print(sign)  
        return sign  
  
    # 发送请求  
    def get_url(self):  
        # 生成时间戳  
        t = str(int(time.time()*1000))  
        data = '{"appId":"34385","params":"{\"devi...}'  
        params = {  
            "jsv":"2.7.4",  
            "appKey":"12574478",  
            "t":t,  
            "sign":self.get_sign(t,data),  
            "api":"mtop.relationrecommend.wirelessrecommend.recommend",  
            "v":"2.0",  
            "type":"get",  
            "timeout":10000,  
            "dataType":"jsonp",  
            "data":data  
        }  
        response = requests.get(self.basic_url,headers = self.headers,params = params).text  
        print(response)  
    # 提取数据  
    def format_data(self):  
        pass  
  
    # 保存数据  
    def save_data(self):  
        pass  
  
if __name__ == '__main__':  
    tb = TaoBao()  
    tb.get_url()
```

经过多次尝试，将源码中的这两段代码替换后，成功的得到了数据。

```python
data_dict = {  
    "appId": "34385",  
    "params": json.dumps({  
        "appId": "34385",  
        "page": 1,  
        "n": 48,  
        "q": "机械键盘",  
        "search_action": "initiative",  
        "sversion": "13.6",  
        "style": "list",  
        "ttid": "600000@taobao_pc_10.7.0",  
        "tab": "all",  
        "sort": "_coefp"  
    }, ensure_ascii=False, separators=(',', ':'))  
}  
data = json.dumps(data_dict, ensure_ascii=False, separators=(',', ':'))  
  
params = {  
    "jsv": "2.7.4",  
    "appKey": "12574478",  
    "t": t,  
    "sign": self.get_sign(t, data),  
    "api": "mtop.relationrecommend.wirelessrecommend.recommend",  
    "v": "2.0",  
    "type": "originaljson",  
    "timeout": "10000",  
    "dataType": "json",  
    "data": data  
}
```

### 问题根源

**主要问题：参数格式不符合API要求，特别是data参数的结构和内容**

原始代码中的data参数结构不符合淘宝API的要求，导致appId验证失败。具体来说：

1. **params字段格式错误**：原始代码中params字段的值是一个JSON字符串，但API期望的是一个嵌套的对象结构
2. **缺少必要参数**：原始参数缺少了一些API必需的字段

### 关键修复点

1. **双重appId**：在data_dict和params中都包含了appId
2. **参数简化**：去掉了大量非必需参数，只保留核心参数
3. **正确的JSON序列化**：使用`json.dumps`确保正确的格式
4. **参数完整性**：添加了必需的参数如`search_action`、`sversion`等
5. `type`参数值应为"originaljson"而不是"get"
6. `dataType`参数值应为"json"而不是"jsonp"
7. `timeout`应该是字符串类型