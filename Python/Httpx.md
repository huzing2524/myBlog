### 【python】Httpx

- [Httpx文档地址](https://link.zhihu.com/?target=https%3A//www.python-httpx.org/) -> A next-generation HTTP client for Python.
- 比requests更加强大的网络请求第三方包，支持同步、异步、HTTP1/2
- 文档的搬运工，目的是记录一下有这个库。
- 同步请求方式和requests包基本相同：
  ```bash
  In [1]: import httpx

  In [2]: r = httpx.get("https://www.sina.com")

  In [3]: r
  Out[3]: <Response [200 OK]>

  In [4]: r.status_code
  Out[4]: 200

  In [5]: r.text[:100]
  Out[5]: '<!DOCTYPE html>\n<html>\n<head>\n\t<title>home.sina.com</title>\n\t<meta charset="utf-8">\n\t<meta name="vie'
  ```
- 异步网络请求 (Ipython3 + Python 3.8.2)：
  ```bash
  In [1]: import httpx

  In [2]: async with httpx.AsyncClient() as client:
  ...:     r = await client.get("https://www.sina.com")

  In [3]: r
  Out[3]: <Response [200 OK]>

  In [4]: r.text[:100]
  Out[4]: '<!DOCTYPE html>\n<html>\n<head>\n\t<title>home.sina.com</title>\n\t<meta charset="utf-8">\n\t<meta name="vie'
  ```
- 异步网络请求在python函数中使用：
  ```python
  import asyncio
  import httpx

  async def main():
      async with httpx.AsyncClient() as client:
          response = await client.get('https://www.sina.com')
          print(response)
          print(response.headers)

  asyncio.run(main())
  ```
