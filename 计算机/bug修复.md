# 解决UnicodeEncodeError: 'ascii' codec can't encode characters in position 18-20: ordinal not in range
在使用GridSearchCV、RandomizedSearchCV进行参数优化之后，继续训练模型时，出现错误：

```python
UnicodeEncodeError: 'ascii' codec can't encode characters in position 18-20: ordinal not in range(128)
```

可能是由于GridSearchCV、RandomizedSearchCV中的参数n_jobs设置为-1了。

解决：把其中的参数：n_jobs=-1改为n_jobs=1即可。

n_jobs：并行计算时使用的计算机核心数量，默认值为1。当n_jobs的值设为-1时，则使用所有的处理器。

在使用交叉验证cross_val_score时，如果出现以上错误，也可以使用同样的方法进行修改。

