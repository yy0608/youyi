---
title: mongoose的地理位置查看附近信息
date: 2018-03-17 14:22:46
tags:
  - 地理位置
---
###### 一、数据的保存，使用2dsphere索引
```
coordinates: {
  type: [ Number ],
  index: {
    type: '2dsphere',
    sparse: true
  }
}
```
###### 二、根据某个位置查询附近，不计算距离
```
MerchantShop.find({
  'location': {
    $nearSphere: coordinates,
    $maxDistance: parseFloat(maxDistance) / 6371
  }
}).limit(limit).skip(skip) // 返回不带距离的数据
  .then(data => {
    res.json({
      success: true,
      msg: '获取附近店铺成功',
      data: data
    })
  })
  .catch(err => {
    res.json({
      success: false,
      mag: '获取附近店铺失败',
      err: err.toString()
    })
  })
```
###### 三、根据某个位置查询附近，计算距离，返回单位米
```
MerchantShop.aggregate([{ // 返回带距离的数据，单位是米
  '$geoNear': {
    'near': {
        'type': 'Point',
        'coordinates': coordinates
      },
    'spherical': true,
    'distanceField': 'distance_m', // 最后生成的距离字段
    'limit': limit
  }
}, { '$skip': skip }]) // 注意此处的limit和skip参数使用
  .then(data => {
    res.json({
      success: true,
      msg: '获取附近店铺成功',
      data: data
    })
  })
  .catch(err => {
    res.json({
      success: false,
      msg: '获取附近店铺失败',
      err: err.toString()
    })
  })
```
