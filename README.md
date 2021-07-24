# Custom-Video-Player
这是用video标签做的自定义播放器
<a href="https://xl-z4869.github.io/Custom-Video-Player/">效果</a>
 
### 一、知识点
#### 1.video
* play() 播放
* pause() 停止
* paused 已经停止
* currentTime 目前播放时间
* duration 全部时间
#### 2.flex-basis
决定了 flex 元素的内容盒（content-box）的尺寸
```
.progress__filled {
    width: 50%;
    background: #ffc600;
    flex: 0;
    flex-basis: 50%;
}
```

### 二、主要步骤
#### 1.获取元素
```
 const player = document.querySelector(".player")
 const video = player.querySelector(".viewer")

 const progress = player.querySelector('.progress')
 const progressBar = player.querySelector('.progress__filled')

 const toggle = player.querySelector('.toggle')
 const ranges = player.querySelectorAll('.player__slider')
 const skips = player.querySelectorAll('.player_skip')
```
#### 2.开始/暂停按钮
```
function clickPlay(){
  let method=video.paused?'play':'pause'
  video[method]()
}

video.addEventListener('click', clickPlay)
toggle.addEventListener('click', clickPlay)
```
#### 3.toggle的样式更改
```
function toggleChange(){
  let icon=video.paused? '►' : '❚ ❚'
  toggle.textContent=icon
}


video.addEventListener('play',toggleChange)
video.addEventListener('pause',toggleChange)
```
#### 4.range事件（声音和速度）
声音和速度的事件处理是一样的，可以将他们写在一起
```
function rangeChange(){
  video[this.name]=value
}

ranges.forEach((range)=>range.addEventListener('change',rangeChange))
ranges.forEach((range)=>range.addEventListener('mousemove',rangeChange))
```
#### 5.skip（前进和后退）
在Html中用data-skip标注好skip代表着多少，在js中用dataset.skip使用
```
function skipClick(){
	video.currentTime+=parseFloat(this.dataset.skip)
}

skips.forEach((skip)=>skip.addEventListener('click',skipClick)
```
#### 6.progress进度条事件
这里需要分两种情况，一种是鼠标直接点击，一种是通过拖拉的方式（也就是点击之后没有放开），因此要记录鼠标按下时候的状态
```
let mousedown=false

function progressChange(e){
  video.currentTime=e.offsetX/progress.offsetWidth*video.duration
}

progress.addEventListener('click',progressChange)
progress.addEventListener('mousemove',(e)=>{
  mousedown&&progressChange(e)
})
progress.addEventListener('mousedown',mousedown=true)
progress.addEventListener('mouseup',mousedown=false)
```
#### 7.peogressBar的样式改变
```
function progressStyle(){
  let persom=(video.currentTime/video.duration)*100
  progressBar.style.flexBasis=`${person}%`
}

video.addEventListener('timeupdate',progressStyle)
```
