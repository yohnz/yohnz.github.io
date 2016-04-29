title: React自学笔记
date: 2016-02-19 18:06:33
tags: React
---

本文记录一些我在自学React过程中遇到的问题

- **getInitialState**

React在ES6中去掉了getInitialState函数，需要写在constructor中

> The API (via 'extends React.Component') is similar to React.createClass with the exception of getInitialState. Instead of providing a separate getInitialState method, you set up your own state property in the constructor.

非ES6写法：

```javascript
var CommentBox = React.createClass({
	getInitialState:function(){
		return {data:[]};
	},
	return:function(){
   	 ......
	}
})
```

ES6写法：
```javascript
class CommentBox extends React.Component{
	constructor(props){
		super(props);
		this.state = {data:[]}
	}
	return(){
		......
	}
}
```

- **No Autobinding**
> Methods follow the same semantics as regular ES6 classes, meaning that they don't automatically bind this to the instance. You'll have to explicitly use .bind(this) or arrow functions =>.

ES6中this需要手动绑定：
```javascript
class CommentForm extends React.Component{
	constructor(props){
		super(props);
		this.handleSubmit = this.handleSubmit.bind(this);
	}
	handleSubmit(e){
		e.preventDefault();
		var author = this.refs.author.value.trim();
		var text = this.refs.text.value.trim();
		if(!author || !text){
			return;
		}
		this.props.onCommentSubmit({author:author,text:text});
		this.refs.author.value = ""
		this.refs.text.value = ""
		return;
	}
	render(){
		return (
			<form className="commentForm" onSubmit={this.handleSubmit}>
				<input type="text" placeholder="your name" ref="author"/>
				<input type="text" placeholder="say something..." ref="text" />
				<input type="submit" value="Post" />
			</form>
		)
	}
}

```