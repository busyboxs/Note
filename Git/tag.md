# git tag comand

## 列出所有标签

```bash
git tag
```

## 添加标签

格式：

```bash
git tag -a tag-name -m "comment"
```

例子：添加一个新的 `tag`，`tag` 名为 `1.2`，说明为新发布版本

```bash
git tag -a 1.2 -m "新发布版本"
```

## 删除标签

```bash
git tag -d tag-name  # 删除本地标签
git push origin :refs/tags/tag-name # 删除远程标签
```

## 上传 tag 到远端

```bash
git push origin tag-name  # 推送指定的标签
git push origin --tags # 推送全部未推送的标签
```