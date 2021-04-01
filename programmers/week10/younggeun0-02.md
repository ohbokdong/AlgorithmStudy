# [스킬트리](https://programmers.co.kr/learn/courses/30/lessons/49993#)

```js
function solution(skill, skill_trees) {
    let answer = 0;

    // skill_trees에서 skill에 포함되지 않는 스킬들 제거
    let filteredTrees = skill_trees.map(tree => {
        return tree.split('').filter(char => skill.includes(char));
    });

    for (let i =0; i<filteredTrees.length; i++) {
        let bValid = true;
        const filteredTree = filteredTrees[i];

        // skill과 filteredTree가 일치해야 valid한 스킬트리 
        for (let j=0; j<filteredTree.length; j++) {
            if (skill[j] !== filteredTree[j]) {
                bValid = false;
                break;
            }
        }

        if (bValid) answer++;
    }

    return answer++;
}
```
