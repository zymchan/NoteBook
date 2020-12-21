```python
import redis
from SeleniumLibrary import LibraryComponent
from robot.api.deco import keyword


class RedisClusterKeyword(LibraryComponent):

    def __init__(self, ctx):
        LibraryComponent.__init__(self, ctx)
        self._redis_url = None
        self._password = None


    @keyword
    def connect_to_redis_cluster(self, redis_url, password=None):
        """连接redis 集群

        例:
        | Connect To Redis Cluster | 10.32.0.150:6600 | ${password} |
        """
        self._redis_url = redis_url
        self._password = password

    @keyword
    def set_to_redis_cluster(self, key, value, expire_time=3600):
        """ Redis 修改key为value，没有则新增key-value
        例：

        | Set To Redis Cluster | key | value |

        """
        try:
            url = self._get_url()
            redis_conn = redis.from_url(url, 0)
            redis_conn.set(key, value, expire_time)
        except Exception as err:
            url = self._get_url_from_err(err)
            redis_conn = redis.from_url(url, 0)
            redis_conn.set(key, value, expire_time)

    @keyword
    def get_from_redis_cluster(self, key):
        """ 获取Redis中key的值

        例:
        | ${value} | Get From Redis Cluster | key |
        """
        try:
            url = self._get_url()
            redis_conn = redis.from_url(url, 0)
            return redis_conn.get(key)
        except Exception as err:
            url = self._get_url_from_err(err)
            redis_conn = redis.from_url(url, 0)
            return redis_conn.get(key)

    @keyword
    def delete_from_redis_cluster(self, key):
        """删除Redis 中Key的键值对

        例:
        | Delete From Redis Cluster | key |
        """
        try:
            url = self._get_url()
            redis_conn = redis.from_url(url, 0)
            redis_conn.delete(key)
        except Exception as err:
            url = self._get_url_from_err(err)
            redis_conn = redis.from_url(url, 0)
            redis_conn.delete(key)

    def _get_url(self):
        return self._generate_connect_url(self._redis_url)

    def _get_url_from_err(self, err):
        self.log(err)
        return self._generate_connect_url(str(err).split(' ')[2])

    def _generate_connect_url(self, url):
        if self._password is None:
            url = "redis://" + url
        elif self._password.find(":") >= 0:
            url = "redis://" + self._password + "@" + url
        else:
            url = "redis://:" + self._password + "@" + url
        return url


```