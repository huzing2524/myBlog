###【Python】numpy.where() 多条件交集、并集

```bash
Python 3.6.8 (default, Jan 14 2019, 11:02:34) 
[GCC 8.0.1 20180414 (experimental) [trunk revision 259383]] on linux

>>> import numpy
>>> a = numpy.arange(10)
>>> a
array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
>>> numpy.where((a > 5) & (a < 8))  # 两个条件同时成立交集 &
(array([6, 7]),)
>>> numpy.where((a > 8) | (a < 5))  # 两个条件同时成立并集 |
(array([0, 1, 2, 3, 4, 9]),)
```