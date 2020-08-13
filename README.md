# axb-checkbox-fix
针对DCloud插件checkbox的修改，原本只有单选或全选，现加入选中数量限制  
#### 增加num参数，为限制的选中数量，默认为1  
```javascript
  num:{
				type:Number,
				default:1
			}
```
#### 超过限制发送事件  
```javascript
  this.$emit('overLimit')
```
#### 多选时的逻辑
```javascript
if(this.items[i].value === val){ // 选中项处理
	if(this.selectarr.length < this.num){		//是否超过限制
		r.checked = !this.items[i].checked
			if(r.checked){				//选中
				this.selectarr.push(this.items[i].value)
			}
			else{						//取消选中
			let no = this.selectarr.indexOf(this.items[i].value)
			if(no>-1){
				this.selectarr.splice(no,1)
			}
			}
	}
	else{									//超过限制
		this.items[i].checked = false		//不选中
		let no = this.selectarr.indexOf(this.items[i].value)
		if(no>-1){							//若当前选择已被选中
			this.selectarr.splice(no,1)
		}
		else{								//发送事件
			this.$emit('overLimit')
		}
	}	
	}
	else{
	r.checked=this.items[i].checked
	}
```
**当multi参数为true时，手动维护selectarr数组。未超过限制时，改变选中样式，维护selectarr数组；超过限制时，一律为未选中样式，同时当当前选择已在数组中时，应该splice。**
