## 🦝 2022-07-18 (월) 
### 오늘 한 것
#### 1. react query로 데이터 패칭시 라인 차트 사라졌다 생성되는 현상 해결
- 라인차트는 원래 echarts-for-react 라이브러리로 컴포넌트를 생성해 사용했었는데 react-query의 isLoading, isFetching이 true일 때마다 loader가 보이게끔 하였다.
- 다른 차트와는 다르게 라인 차트는 isFetching을 사용하지 않으면 10초마다 변경되는 그래프가 처음부터 생성되는 것이 아닌, 원래의 그래프에서 형태만 바뀌는 애니메이션이었다.
- 그런데 echarts-for-react의 ReactECharts 말고 div에 ref 걸어서 echarts 사용하니 패칭되어도 loader가 보이지 않으며 데이터 패칭시마다 그래프가 처음부터 생성되었다.
- 왜 그런지는 자세히 모르겠으나 init, clear 함수를 계속 사용해야 하는 것 보면 컴포넌트가 계속 새로 생성되는 듯 하다...

#### 2. ReactECharts 말고 useRef로 특정 DOM에 echarts 렌더링시 theme 적용하는 방법 터득 
```js
import * as echarts from 'echarts';
import '@styles/echart-theme' // theme 위치한 곳

const chartRef = useRef<any>(null);
if (chartRef.current) {
      const chart = echarts.init(chartRef.current, 'chartTheme');
      chart.setOption(result);
}
// init할 때 theme 같이 적용
```   






