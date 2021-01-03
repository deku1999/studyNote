## 滚动视图组件ScrollView

滚动视图组件，顾名思义，就是当内容超过指定的高度时会可以通过滑动来显示，右边还会显示滚动条。`ScrollView`的使用很简单，只要包括在要滚动的组件外面就可以了。

例如上面那个范例，我们只需要做一点点的修改

```react
import React, { Component } from 'react';
import { Text, View, ScrollView, StyleSheet} from 'react-native';

class App extends Component {
   state = {
      languages: [
         {'name': 'Python', 'id': 1},
         {'name': 'Perl', 'id': 2},
         {'name': 'PHP', 'id': 3},
         {'name': 'Ruby', 'id': 4},
         {'name': 'Scala', 'id': 5},
         {'name': 'JavaScript', 'id': 6},
         {'name': 'Rust', 'id': 7},
         {'name': 'Go', 'id': 8},
         {'name': 'Java', 'id': 9},
         {'name': 'C++', 'id': 10},
         {'name': 'C', 'id': 11},
         {'name': 'Awk', 'id': 12},
         {'name': 'Sed', 'id': 13},
         {'name': 'TypeScript', 'id': 14},
         {'name': 'C#', 'id': 15},
         {'name': 'F#', 'id': 16},
         {'name': 'CSS', 'id': 17},
         {'name': 'HTML', 'id': 18},
         {'name': 'React Native', 'id': 19}
      ]
   }
   render() {
      return (
         <View style={styles.list}>
            <ScrollView>
               {
                  this.state.languages.map((item, index) => (
                     <View key = {item.id} style = {styles.item}>
                        <Text>{item.name}</Text>
                     </View>
                  ))
               }
            </ScrollView>
         </View>
      )
   }
}
export default App
```

<video controls="" src="https://www.twle.cn/static/upload/img/2019/07/09/20190709203738_4.mp4" style="box-sizing: border-box; display: inline-block; vertical-align: baseline;"></video>

## 选择器Picker

如果要从多个 **已知的选项** 中选择一个，那么可以使用 React Native 内置的 **选择器 **`Picker`。

选择器默认显示如下

![React Native 选择器 Picker](https://www.twle.cn/static/upload/img/2019/07/10/20190710200633_4.png)

当被点击时显示如下

![React Native 选择器 Picker](https://www.twle.cn/static/upload/img/2019/07/10/20190710200752_4.png)

### 使用语法

```react
<Picker 
   selectedValue = "male"
   onValueChange = {updateUser} >
    //label为显示出来的名称，value为选项的值
   <Picker.Item label = "男"   value = "male" />
   <Picker.Item label = "女"   value = "female" />
   <Picker.Item label = "其它"  value = "other" />
</Picker>
```

| 属性          | 说明                           |
| ------------- | ------------------------------ |
| selectedValue | 用于设置默认的选中项目         |
| onValueChange | 用于设置选中项变更时的触发操作 |

**onValueChange属性**

`onValueChange` 属性用于设置选项变更时触发的操作。`onValueChange` 属性的触发的事件原型如下

```react
//itemValue为选中的value值，itemPosition为选中项的位置，第一个位置为0
function(itemValue,itemPosition) {
// 具体的处理逻辑
}
```

## 网络请求

### get请求

```react
requestToTest() {
        return fetch(apiURL, {
            method: 'GET',
        }) 
        .then((response) => response.json())
        .then((data) => {
            console.log(data);
            return data;
        }).catch((err) => {
            console.log(err);
        });
    }
```

### post请求

```react
requestToGetApplyId() {
        return fetch(apiURL, {
            method: 'POST',
            headers: {
                'Authorization': 'Bearer ' + token,
                'Accept': 'application/json',
                'Content-Type': 'application/json',
            },
            body: JSON.stringify({
                'name': 'userName',
                'telephone': '18088888888'
            })
        }) 
        .then((response) => response.json())
        .then((data) => {
            console.log(data);
            return data;
        }).catch((err) => {
            console.log(err);
        });
    }
```

### put请求

```react
requestToLogin(account, password) {

        return fetch(apiURL, {
            method: 'PUT',
            headers: {
                'Accept': 'application/json',
                "Content-Type": "application/x-www-form-urlencoded"
            },
            body: `userName=${userName}&passWord=${passWord}`
        })
        .then((response) => response.json())
        .then((data) => {
            console.log(data);
            return data;
        }).catch((err) => {
            console.log(err);
```

### 案例

```react
import React, {Component} from 'react';
import {
  StyleSheet, 
  Text, 
  Image,
  View
} from 'react-native';

var REQUEST_URL =
  "https://raw.githubusercontent.com/facebook/react-native/0.51-stable/docs/MoviesExample.json";

export default class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      movies: null,
    };
    this.fetchData = this.fetchData.bind(this);
  }

  componentDidMount() {
    this.fetchData();
  }

  fetchData() {
    fetch(REQUEST_URL)
      .then((response) => response.json())
      .then((responseData) => {
        this.setState({
          movies: responseData.movies,
        });
      });
  }

  render() {
    if(!this.state.movies) {
      return this.renderLoadingView();
    }
    var movie = this.state.movies[0];
    return this.renderMovie(movie);
  }

  renderLoadingView() {
    return (
      <View style = {styles.container}>
        <Text>loading...</Text>
      </View>
    );
  }

  renderMovie(movie) {
    return(
      <View style = {styles.container}>
        <Image 
          style = {styles.thumbnail}
          source = {{uri: movie.posters.thumbnail}}
        />
        <View style = {styles.rightContainer}>
          <Text style = {styles.title}>{movie.title}</Text>
          <Text style = {styles.year}>{movie.year}</Text>
        </View>
      </View>
    );
  }
}
```

