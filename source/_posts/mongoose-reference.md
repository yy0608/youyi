---
title: mongoose关联查询
tags:
  - mongoose关联查询
abbrlink: 48025
date: 2018-02-05 16:17:55
---
##### 博客开发中帖子和分类的关联查询，经验记录
###### 一、初始化两个schema和model
```
// categories.js     schema
export default new mongoose.Schema({
  name: String,
  desc: String
})

// Category.js        model
export default mongoose.model('Category', categoriesSchema)


// topics.js          schema
export default new mongoose.Schema({
  category: {
    type: mongoose.Schema.types.ObjectId,
    ref: 'Category'
  },
  title: {
    type: String
  }
})

// Topic.js            model
export default mongoose.model('Topic', topicsSchema)
```
###### 二、使用populate关联查询，以下写法任选一
```
Topic.find().populate('category')

Topic.find().populate({
  path: 'category'
})

Topic.find().populate({
  path: 'category',
  select: 'name'
})

Topic.find().populate({
  path: 'category',
  select: {
    _id: 0,
    name: 1
  }
})
```
