# [N으로 표현](https://programmers.co.kr/learn/courses/30/lessons/42895)

```JAVASCRIPT
function solution(N, number) {
    const set = new Array(8).fill().map(() => new Set());
    // [set() for x in range(8)]
    for(let i=0; i<8; i++){
        set[i].add(Number(N.toString().repeat(i+1)));
        console.log(set[-1])
        for (let j=0; j < i; j++) {
            for(const arg1 of set[j]){
                for(const arg2 of set[i-j-1]){
                    set[i].add(arg1+arg2);
                    set[i].add(arg1-arg2);
                    set[i].add(arg1*arg2);
                    set[i].add(arg1/arg2);
                }
            }
        }
        if(set[i].has(number)) return i+1;
    }
    return -1;
}
```
```JAVASCRIPT
function solution(N, number) {
  const s = [...Array(8)].map((v, i) => new Set([parseInt(Array(i + 1).fill(N).join(''))]));
  if (N == number) return 1;
  for (let i = 1; i < 8; ++i) {
    for (let j = 0; j < i; ++j) {
      s[j].forEach(oper1 => {
        s[i - j - 1].forEach(oper2 => {
          s[i].add(oper1 * oper2);
          s[i].add(oper1 + oper2);
          s[i].add(oper1 - oper2);
          
          if (oper2) s[i].add(Math.floor(oper1 / oper2));
        })
      })
    }
    
    if (s[i].has(number)) return i + 1;
  }
  return -1;
}
```
일단 코드는 가지고 왔는데 설명은 필요함