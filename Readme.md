# Typing-Game 打字游戏

## 引入animate.js，通过animate方法完成动画

## 面向对象模式
```
Game.prototype = {
		// 开始动画
		action:function(){
        ...
		},
		// 开始游戏
		start:function(){
				...
		},
		// 创建div
		createDiv:function(num){
				...
		},
		// div下落
		divDown:function(speed,some,num,fn){
				...
		},
     // div消失
		deleteDiv:function(some,num,fn){
			  ...
		},
     // 中级模式
		mediumfn:function(){
				...
		},
		// 困难模式
		difficult:function(){
				...
		},
		// 结束
		success:function(){
				...
		}
}
```

## 创建num个div，div数量受关卡难度控制，div里随机填入英文字母，div的位置随机
```
this.letterlist = ['A','B','C','D','E','F','G','H','I','J','K','L','M','N','O','P','Q','R','S','T','U','V','W','X','Y','Z']

createDiv:function(num){
    // 创建num个
		for(var i = 0;i < num;i++){
				var div = document.createElement('div');
         // 随机位置
				div.style.left = Math.floor(Math.random()*1000) + 100 + 'px';
				div.style.top = 0;
				div.className = 'letter';
         // 随机字母
				div.innerHTML = this.letterlist[Math.floor(Math.random()*26)];
				this.box_top.appendChild(div);
		}
},
```

## 通过键盘事件，寻找当前按键的code码对应的字母，若与飘落字母对应，则删除其所在div，并创建新的div
```
deleteDiv:function(some,num,fn){
// 以参数形式传入三个关卡的分数值、三个关卡同时出现的div数(count)、通关动画
				var _this = this;
				document.onkeydown = function(e){
					var e = e || event;
          // 转换ASCLL码
					var keyStr = String.fromCharCode(e.keyCode);	
					for(var i = 0;i < _this.divs.length;i++){
						if(keyStr === _this.divs[i].innerHTML){
						// 效果：当按下一个键时，这个键对应的盒子消失，同时生成一个随机盒子
							_this.divs[i].remove();
							_this.count--;
							_this.score++;
							...
              // 一个div消失，创建新的div
							if(_this.count == num-1){
								_this.createDiv(1);
								_this.count = num;
							}
						} 
					}
				}
			},
```

## 通过animate方法，控制div下落动画，div下落的速度值为speed受关卡难度影响
```
divDown:function(speed,some,num,fn){
	// 速度值、deleteDiv的参数
	this.divs = document.getElementsByClassName('letter')
	for(var i = 0;i < this.divs.length;i++){
		var top = parseInt(this.divs[i].style.top);  // 获取初始值，得到字符串，parseInt强制转换
		if(top >= this.Height-50){
			...
			animate(this.again,{top:100},500);
			...
			this.count = num;
			clearInterval(this.t);
		} else {
			this.result = top + speed; // 速度值
			this.divs[i].style.top = this.result + 'px';
		}				
	};
	this.deleteDiv(some,num,fn);
},
```
