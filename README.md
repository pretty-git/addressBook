## 先上效果图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200324103311381.gif)
## 样式是采用的colorUI里的，难点在如果把后台返回来的数据按照首字母拼音去分类，主要是获取拼音的首字母！！！
上传一部分关键代码，需要全部代码的可以去我的github上拉取；[点击查看我的GitHub](https://github.com/pretty-git/addressBook.git)
https://github.com/pretty-git/addressBook.git

```javascript
Page({
  data: {
    classId: '',
    StatusBar: getApp().globalData.StatusBar,
    CustomBar: getApp().globalData.CustomBar,
    hidden: true,
    datalist: [
      {
        parentsName:'张大炮',
        headPortrait:'https://dss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2471723103,4261647594&fm=26&gp=0.jpg',
        parentsContactWay:'12345678910'
      },
      {
        parentsName: '王立',
        headPortrait: 'https://dss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2471723103,4261647594&fm=26&gp=0.jpg',
        parentsContactWay: '12345678910'
      },
      {
        parentsName: '徐东',
        headPortrait: 'https://dss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2471723103,4261647594&fm=26&gp=0.jpg',
        parentsContactWay: '12345678910'
      },
      {
        parentsName: '陈里',
        headPortrait: 'https://dss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2471723103,4261647594&fm=26&gp=0.jpg',
        parentsContactWay: '12345678910'
      },
      {
        parentsName: '刘强',
        headPortrait: 'https://dss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2471723103,4261647594&fm=26&gp=0.jpg',
        parentsContactWay: '12345678910'
      },
      {
        parentsName: '苟小刚',
        headPortrait: 'https://dss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2471723103,4261647594&fm=26&gp=0.jpg',
        parentsContactWay: '12345678910'
      },
      {
        parentsName: '汪苏泷',
        headPortrait: 'https://dss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2471723103,4261647594&fm=26&gp=0.jpg',
        parentsContactWay: '12345678910'
      }, {
        parentsName: '何少',
        headPortrait: 'https://dss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2471723103,4261647594&fm=26&gp=0.jpg',
        parentsContactWay: '12345678910'
      },
      {
        parentsName: '贾坤',
        headPortrait: 'https://dss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2471723103,4261647594&fm=26&gp=0.jpg',
        parentsContactWay: '12345678910'
      },
      {
        parentsName: '川哥',
        headPortrait: 'https://dss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2471723103,4261647594&fm=26&gp=0.jpg',
        parentsContactWay: '12345678910'
      }
    ],
    leadlist: [],
    list: '',
    listCur: '',
    parentName: '',
  },
  onLoad(options) {
    this.setData({
      classId: options.classId
    })
    this.loadAll()
  },
  loadAll() {
    let list = [];
    for (let i = 0; i < 26; i++) {
      list[i] = String.fromCharCode(65 + i)
    }

    for (var item of this.data.datalist) {
      var value = item.parentsName.substring(0, 1)
      item.letter = pinyin.getCamelChars(value)
    }
    var lists = list
    for (var i = 0; i < lists.length; i++) {
      var book = 0
      for (var j = 0; j < this.data.datalist.length; j++) {
        if (lists[i] == this.data.datalist[j].letter) {
          book = 1
        }
      }
      if (book == 1) {
        this.data.leadlist.push(lists[i])
      }
    }
    this.setData({
      leadlist: this.data.leadlist,
      datalist: this.data.datalist,
      list: list,
      listCur: list[0]
    })
  },
  search(e) {  //搜索
    this.setData({
      parentName: e.detail.value
    })

  },
  searchbtn() {
    this.setData({
      leadlist: [],
      datalist: []
    })
    this.loadAll()
  },
  call(e) {   //打电话
    var tel = e.currentTarget.dataset.tel
    wx.makePhoneCall({
      phoneNumber: tel
    })
  },
  onReady() {
    let that = this;
    wx.createSelectorQuery().select('.indexBar-box').boundingClientRect(function (res) {
      that.setData({
        boxTop: res.top
      })
    }).exec();
    wx.createSelectorQuery().select('.indexes').boundingClientRect(function (res) {
      that.setData({
        barTop: res.top
      })
    }).exec()
  },
  //获取文字信息
  getCur(e) {
    this.setData({
      hidden: false,
      listCur: this.data.list[e.target.id],
    })
  },

  setCur(e) {
    this.setData({
      hidden: true,
      listCur: this.data.listCur
    })
  },
  //滑动选择Item
  tMove(e) {
    let y = e.touches[0].clientY,
      offsettop = this.data.boxTop,
      that = this;
    //判断选择区域,只有在选择区才会生效
    if (y > offsettop) {
      let num = parseInt((y - offsettop) / 20);
      this.setData({
        listCur: that.data.list[num]
      })
    };
  },

  //触发全部开始选择
  tStart() {
    this.setData({
      hidden: false
    })
  },

  //触发结束选择
  tEnd() {
    this.setData({
      hidden: true,
      listCurID: this.data.listCur
    })
  },
  indexSelect(e) {
    let that = this;
    let barHeight = this.data.barHeight;
    let list = this.data.list;
    let scrollY = Math.ceil(list.length * e.detail.y / barHeight);
    for (let i = 0; i < list.length; i++) {
      if (scrollY < i + 1) {
        that.setData({
          listCur: list[i],
          movableY: i * 20
        })
        return false
      }
    }
  },
});
```
