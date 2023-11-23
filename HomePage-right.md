> 使用指南 [玩转 Obsidian 08：利用 Dataview 打造自动化 HomePage - 少数派](https://client.sspai.com/post/73958)
## 1 Daily
展示最新的「日记」

```dataview
	LIST
	From "02 笔记/日记"
	sort file.mtime desc limit (7)
```

### 1.1 最近修改
展示最近10天内修改的五篇笔记

```dataview
	LIST WHERE file.mtime >= date(today) - dur(10 day) sort file.mtime desc limit (5)
```

## 2 稍后读
在 Obsidian 中通过 Todo 管理「稍后读」，任何一篇笔记中如果想将一篇文章进行「稍后阅读」，直接创建一条 Todo，并添加 `#稍后读` 标签即可

```dataviewjs
	dv.table(["Task","Name"],

		dv.pages("#稍后读").file.tasks

		.where(t => (!t.completed && t.text.indexOf("#稍后读")>0))

		.map(b => ["[ ] - " + b.text,b.link])

	)
```

## 3 闪念胶囊 Todo
```dataviewjs
	dv.table(["Task","Name"],
		dv.pages("#闪念胶囊").file.tasks
		.where(t => (!t.completed && t.text.indexOf("#闪念胶囊")>0))
		.map(b => ["[ ] - " + b.text,b.link])
	)
```

## 4 podcast
```dataviewjs
	dv.table(["Name","Type","Create Time"],
		dv.pages("#PodCast and #waiting")
		.map(b => [b.file.link,b.type,b.file.ctime])
	)
```

## 5 其他未完成任务
```dataviewjs
	dv.table(["Task","Name"],
		dv.pages("-#闪念胶囊 and -#稍后读")
		.file.tasks.where(t => !t.completed)
		.limit(10)
		.map(b => ["[ ] - " + b.text,b.link])
	)
```


