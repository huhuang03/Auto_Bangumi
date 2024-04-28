About auto complete

The code is:
```js
configKey: 'eps_complete'
```

```python
def eps_complete():
    with RSSEngine() as engine:
        datas = engine.bangumi.not_complete()
        if datas:
            logger.info("Start collecting full season...")
            for data in datas:
                if not data.eps_collect:
                    with SeasonCollector() as collector:
                        collector.collect_season(data)
                data.eps_collect = True
            engine.bangumi.update_all(datas)
            
def not_complete(self) -> list[Bangumi]:
    # Find eps_complete = False
    # use `false()` to avoid E712 checking
    # see: https://docs.astral.sh/ruff/rules/true-false-comparison/
    condition = select(Bangumi).where(
        and_(Bangumi.eps_collect == false(), Bangumi.deleted == false())
    )
    datas = self.session.exec(condition).all()
    return datas
```


