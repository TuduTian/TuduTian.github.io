## 倒计时
```ts
  import { ref, toRaw, markRaw } from 'vue'
  /**
 * @params date 未来时间   beforeCb 开始计时之前的回调函数   afterCb 倒计时已经结束之后的回调函数
 * @return 返回 h s m 以及开启计时和关闭计时的函数
 * 
 */
  const useCountDown = (date, beforeCb, afterCb) => {
    const _futureDate = new Date(date).getTime() // 未来的时间的时间戳
    const h = ref()
    const m = ref()
    const s = ref()
    let time = null

    // 得到  h m s
    const getHMS = () => {
      const _nowDate = new Date().getTime() // 当前时间的时间戳
      const _cha = (_futureDate - _nowDate) / 1000
      if (_nowDate >= _futureDate) {
        clearInterval(time)
        afterCb && afterCb(h, m, s)
        return
      }
      h.value = Math.floor(_cha / 60 / 60);
      m.value = Math.floor(_cha / 60 % 60)
      s.value = Math.floor(_cha % 60)
    }

    // 开始执行
    const run = () => {
      time = setInterval(() => {
        beforeCb && beforeCb()
        getHMS()
      }, 1000);
    }

    // 停止计时
    const stop = () => {
      clearInterval(time)
    }
    return {
      h, m, s,
      run,
      stop,
    }
  }
  useCountDown('2022-08-23 12:00:00')
  export default useCountDown
```