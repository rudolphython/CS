# 캐시(cache)

**CS 핵심이론인 캐시의 개념과 적용 방법입니다.**



## 배경

웹 사이트에서 사용자의 요청이 많아지면 웹 어플리케이션은 많은 부하가 걸리게 되어 있습니다. 사용자의 응답 시간은 길어지고 당연히 사용자의 불편함은 증가합니다. 더 많은 요청을 처리하고 효율적으로 운영하는 방법은 여러 방법이 있지만 그 중 하나의 방법은 **[캐시]** 입니다. 



## 사용 이유

캐시(cache)는 '미리 기억해두었다가 사용하는 것', '가져오는데 비용을 줄이는 것'이라고 알고 있으면 됩니다. 예를 들어, 로그인을 할 때, 기본적으로 쿼리를 파싱하고 해당 유저 id의 인덱스를 찾느라 성능이 저하됩니다. 하지만, 캐시를 사용하게 되면 쿼리를 파싱하지 않아도 되고 접근 속도가 빨라집니다. 추가로, 캐시를 사용하는 방법(HTTP Reverse Proxy Caching, Web Page Caching 등), 캐시를 저장할 곳(메모리, DB 등) 등 다뤄야할 여러 이슈들도 존재합니다.



## 캐시 솔루션 'Redis'

레디스는 key-value 저장소로 NoSQL입니다. 이미 많은 국내 대기업들이 사용 중인 오픈소스 솔루션이며 여러 데이터를 다양한 타입으로 저장 가능합니다. 



## Django에 Redis 설정하기

1. redis 설치

```bash
$ pip install django-redis
```



2. settings.py에 아래코드 작성

```python
CACHES = {
    'default': {
        'BACKEND': 'django_redis.cache.RedisCache',
        'LOCATION': 'redis://{URL}:6379',
    },
}
```



3. 캐시 사용하기

* cache.get()을 통해 캐시에 접근하고, cache.set()을 통해 캐시를 저장하는데 유효 기간을 3600초(1시간, 60초 * 60초)로 설정해두었기 때문에 1시간 이후 해당 데이터는 만료됩니다.

```python
from django.core.cache import cache


def get_post_count():
        cache_key = 'my_count'
        count = cache.get(cache_key, None)
        if not count:
            count = self._get_post_count()
            cache.set(cache_key, count, 60 * 60)
        return count
```





### 참고

캐시 배경

* https://charsyam.wordpress.com/2016/07/27/%EC%9E%85-%EA%B0%9C%EB%B0%9C-%EC%99%9C-cache%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94%EA%B0%80/

Redis 설정

* https://lee-seul.github.io/django/2019/05/02/django-cache-framework.html