# Fang_Scrapy_redis
基于Scrapy_redis实现分布式爬虫，搜房网项目。

url分析
1 获取所有的城市的url链接
https://www.fang.com/SoufunFamily.htm
2 获取所有城市的新房的url
3 获取所有城市的二手房url
例	六安新房 https://luan.newhouse.fang.com/house/s/    六安二手房 https://luan.esf.fang.com/
特例 北京新房 https://newhouse.fang.com/house/s/    北京二手房 https://esf.fang.com/

爬虫脚本sfw.py介绍
定义搜房网爬虫类 SfwSpider，继承自（RedisSpider）
class SfwSpider(RedisSpider):
  # start_url 改成redis——key
  redis_key = "fang:start_urls"
  
  def parse(self,response):  # 解析城市列表页 获取省份城市名以及制作各城市新房二手房的url连接
        yield scrapy.Request(url=newhouse_url, callback=self.parse_newhouse, meta={'info': (province, city)})
           # 发送请求携带参数 meta 传入info元组（province，city)
           #    yield scrapy.Request(url=esf_url, callback=self.parse_esf, meta={'info': (province, city)})
  # 新房数据
  def parse_newhouse(self, response):
  
  # 解析二手房数据
   def parse_esf(self, response):
   
  
  items.py 
import scrapy
class NewhouseItem(scrapy.Item):
省份 城市 小区名 价格 居室 面积 地址 行政区 是否在售 详情页
class EsfItem(scrapy.Item):
省份 城市 小区名 价格 居室 面积 地址 朝向 单价 总价 建成年代 详情页


middleware.py
设置随机请求头
import random
class UserAgentDownloadMiddleware(object):
    # user-agent随机请求头中间件
    USERAGENTS = [ ]  

    def process_request(self, request, spider):
        user_agent = random.choice(self.USERAGENTS)
        request.headers['User-agent'] = user_agent
