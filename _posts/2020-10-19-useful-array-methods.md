---
layout: post
title: "9. 유용한 배열 함수들"
tags: [javascript, array, api, methods]
excerpt_separator: <!--more-->

---

## 문제풀이!

array에서 정렬, 병합, 필터링이 필요할 경우 무작정 `for`, `if` 문을 남발하지 말고 이미 만들어져 있는 API들을 활용하여 함수형 프로그래밍에 집중하자!

<!--more-->

```javascript
// Q1. make a string out of an array
{
  const fruits = ['apple', 'banana', 'orange'];
  
  const result = fruits.join()
}

// Q2. make an array out of a string
{
  const fruits = '🍎, 🥝, 🍌, 🍒';
  
  const result = fruits.split()
}

// Q3. make this array look like this: [5, 4, 3, 2, 1]
{
  const array = [1, 2, 3, 4, 5];
  
  const result = array.reverse()
}

// Q4. make new array without the first two elements
{
  const array = [1, 2, 3, 4, 5];
  
  const result = array.slice(2, 5) // slice는 exclusive하다(마지막 값은 호출되지 않음)
}

class Student {
  constructor(name, age, enrolled, score) {
    this.name = name;
    this.age = age;
    this.enrolled = enrolled;
    this.score = score;
  }
}
const students = [
  new Student('A', 29, true, 45),
  new Student('B', 28, false, 80),
  new Student('C', 30, true, 90),
  new Student('D', 40, false, 66),
  new Student('E', 18, true, 88),
];

// Q5. find a student with the score 90
{
  const result = students.find( (student) => student.score === 90)
}

// Q6. make an array of enrolled students
{
  const result = students.filter( (student) => student.enrolled )
}

// Q7. make an array containing only the students' scores
// result should be: [45, 80, 90, 66, 88]
{
  const result = students.map( (student) => student.score )
}

// Q8. check if there is a student with the score lower than 50
{
  const result1 = students.some( (student) => student.score < 50)
  const result2 = !students.every( (student) => student.score >= 50)
}

// Q9. compute students' average score
{
  const result = students.reduce((prev, curr) => prev + curr.score, 0) / students.length
}

// Q10. make a string containing all the scores
// result should be: '45, 80, 90, 66, 88'
{
  const result = students.map((student) => student.score).join()
}

// Bonus! do Q10 sorted in ascending order
// result should be: '45, 66, 80, 88, 90'
{
  const result = students.map((student) => student.score).sort((a, b) => a - b).join()
  // 점수가 큰거부터 나오게 하려면
  const result = students.map((student) => student.score).sort(a, b) => b - a).join()
}

```

