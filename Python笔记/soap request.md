```python

    @keyword
    def suds_soap_request(url, param, method):
        """ 用suds的包中的方法发送soap 请求,此方法只适用于http请求，https不支持。

        参数说明:

            - url: 请求的地址信息 ：eg. http://127.0.0.1:9006/ui/services/xxxService?wsdl
            - param: 请求的报文参数，不需要加上soapenv的相关tag信息，直接传即可 ：eg. <TradeData><BaseInfo><TaskCode>100206</TaskCode><RequestDate>2019-01-02 11:41:14</RequestDate><ResponseDate></ResponseDate></BaseInfo><Parameters><Parameter><Code></Code><Value></Value></Parameter><Parameter><Code></Code><Value></Value></Parameter></Parameters></TradeData>
            - method: 请求所用的方法， eg. ContTransfer


        例：
        | ${result} | soap request | ${url} | ${param} | ${method} |
        """
        print("request url=%s," % str(url))
        print("request method=%s," % str(method))
        print("request param=%s," % str(param))
        if method =='ContTransfer':
            response = Client(url).service.ContTransfer(param)
        elif method =='service':
            response = Client(url).service.service(param)
        else:
            response = eval("Client(url).service." + method + "(param)")
        print("response=%s," % str(response))
        return response

    @keyword
    def soap_request(url, param, method):
        """用http request的方式发送soap请求
        参数说明:

            - url: 请求的地址信息 ：eg. http://127.0.0.1:9006/ui/services/xxxService?wsdl
            - param: 请求的报文参数，不需要加上soapenv的相关tag信息，直接传即可 ：eg. <TradeData><BaseInfo><TaskCode>100206</TaskCode><RequestDate>2019-01-02 11:41:14</RequestDate><ResponseDate></ResponseDate></BaseInfo><Parameters><Parameter><Code></Code><Value></Value></Parameter><Parameter><Code></Code><Value></Value></Parameter></Parameters></TradeData>
            - method: 请求所用的方法， eg. ContTransfer


        例：
        | ${result} | soap request | ${url} | ${param} | ${method} |
        """
        headers = {
            'Content-type': "text/xml; charset=UTF-8",
            'SOAPAction': ""
        }
        param = __get_soap_request_param(param,method)
        print("request headers=%s," % str(headers))
        print("request body=%s," % str(param))
        param = param.encode('utf-8')
        response = requests.request("POST", url, data=param, headers=headers)
        result = unescape(response.text)
        print("response=%s" % str(result))
        return result


    def __get_soap_request_param(self,param,method):
        pre = '<soapenv:Envelope xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"' \
              ' xmlns:tas="http://taskservice.lis.sinosoft.com"><soapenv:Header/><soapenv:Body><tas:'+method+' soapenv:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/">' \
              '<XMLInputString xsi:type="xsd:string"><![CDATA['
        append = ']]></XMLInputString></tas:'+method+'></soapenv:Body></soapenv:Envelope>'
        return pre + param + append
```