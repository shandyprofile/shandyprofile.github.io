---
title: "Systems of Linear: two variables"
description: >-
  Mermaid demo
author: [shandy]
date: 2025-09-05
categories: [Machine Learning, Linear Algebra for Machine Learning]
tags: [Machine Learning, Linear Algebra for Machine Learning]
sort_index: 101
# pin: true
# media_subpath: '/posts/01'
---

## I. Systems of Equations and Sentences
### 1. “Linear algebra is systems of linear equations.”

Trong ML, hầu hết các mô hình đều được mô tả bằng linear algebra:
- Input (features) ⇒ vector
- Parameters (weights) ⇒ vector/matrix
- Predictions ⇒ kết quả từ linear equations

Ví dụ:

$$
y = w_1 x_1 + w_2 x_2 + \cdots + w_n x_n + b
$$

![](/assets/img/2025-09-08-22-27-55.png)

> Đây chính là một **systems of linear equations** (mô hình linear regression cơ bản).

### 2. “When you think of equations, you think of sentences.”

- Một equation giống như một sentence trong ngôn ngữ: nó truyền đạt một thông tin về dữ liệu.
- Trong ML: mỗi phương trình biểu diễn một constraint (ràng buộc) hoặc một data point.

Ví dụ:
  - Phương trình (Equation): 2x + y = 5
  - Sentence: “Two apples and one banana cost 5 dollars.”
  - Trong ML: Đây là một dữ liệu huấn luyện (x,y).

### 3. “Sentences that are giving you information about things in the world.”

- Trong ngôn ngữ: mỗi câu mô tả một sự thật.
- Trong ML: mỗi data point (sample) cũng là một “câu” về thế giới thực.

Ví dụ::
  - Sentence 1: “Nam is taller than Lan.”
  - Sentence 2: “Lan is 160 cm tall.”
  
  ⇒ We can infer: “Nam is taller than 160 cm.”

$$
\begin{cases}
A > B \\
B = 160
\end{cases}
\quad \Rightarrow \quad A > 160
$$

### 4. “Systems of sentences behave a lot like systems of equations.”

- Ghép nhiều câu lại, ta hiểu rõ hơn bức tranh toàn cảnh.
- Trong ML: nhiều data points (training set) tạo nên một system of equations mà mô hình phải giải để tìm tham số phù hợp.
  
Ví dụ (linear regression với 2 điểm dữ liệu):

$$
\begin{cases}
2x + y = 5 \\
x - y = 1
\end{cases}
$$

> Giống như trong ML, bạn có nhiều mẫu huấn luyện, và bạn cần tìm nghiệm (weights) phù hợp với tất cả.

### 5. “Systems of sentences combine themselves to give you more information.”

- Khi các câu kết hợp, ta có thể suy ra điều mới.
- Trong ML, khi nhiều data points kết hợp, mô hình tổng quát hóa để đưa ra dự đoán cho dữ liệu chưa thấy.

Ví dụ:

- Training data (sentences):
  - “If it rains, the ground gets wet.”
  - “It is raining today.”

⇒ Kết hợp: “The ground will be wet today.”

>Trong ML: mô hình học từ dữ liệu, rồi dự đoán cho input mới.

### 6. Example for system of sentences

![](/assets/img/2025-09-08-22-19-07.png)

---

![](/assets/img/2025-09-08-22-19-14.png)
> Hệ thống này chứa nhiều thông tin như **sentences** và được gọi là **a complete system**.

---

![](/assets/img/2025-09-08-22-20-21.png)
> **The sentences** lặp lại và do đó hệ thống này được gọi là dư thừa (**redundant**).

---

![](/assets/img/2025-09-08-22-21-10.png)
> Hệ thống này được gọi là **contradictory system** vì con chó không thể vừa đen vừa trắng cùng một lúc, hãy nhớ rằng chúng ta có một con chó và nó chỉ có thể có một màu.

---

![](/assets/img/2025-09-08-22-21-34.png)
> Non-singular: hệ có nghiệm duy nhất (complete system).
> Singular: hệ có vô số nghiệm (redundant system) hoặc hệ vô nghiệm (contradictory).

---

## II. System of Linear Equations

### 1. From Sentences to Equations

Quy tắc chuyển đổi

- S1: Xác định thực thể (danh từ) → biến số cần tìm.
- S2: Chọn kiểu biến:
  - Số thực / nguyên → biến x,y.
  - Thuộc tính phân loại (màu, trạng thái) → mã hóa nhị phân (one-hot, 0–1).
  - Logic/Boolean → biến nhị phân (0 hoặc 1).

- S3: Dịch quan hệ (động từ, số lượng) thành ràng buộc toán học:
  - bằng (=),
  - nhiều hơn / ít hơn (≥,≤)
  - “một trong hai” → tổng bằng 1.
- S4: Thêm ràng buộc miền giá trị (ví dụ: số nguyên, không âm, nhị phân).
- S5: Phân tích hệ:
  - Đủ phương trình, det(𝐴) ≠ 0 → nghiệm duy nhất.
  - Thiếu phương trình hoặc ràng buộc dư thừa → vô số nghiệm.
  - Mâu thuẫn → vô nghiệm.

#### 1.1. Ví dụ 1

![](/assets/img/2025-09-08-23-19-57.png)

- Câu: “Between the dog and the cat, one is black”

$$
\begin{cases}
d_B = 1, \text{ If the dog is black, else 0} \\
d_O = 1, \text{ If the dog is organce} \\
c_B, c_O \text{ Similar for cat}
\end{cases}
\quad \Rightarrow \quad
\begin{cases}
d_B + d_O = 1 \\
c_B + c_O = 1
\end{cases}
$$

- Câu: Each animal has only one color.
$$
\text{Each animal has only one color.}
\quad \Rightarrow \quad
d_B + c_B = 1
$$

$$
\quad \Rightarrow \quad
\begin{cases}
d_B + d_O = 1 \\
c_B + c_O = 1 \\
d_B + c_B = 1
\end{cases}
$$

---

#### 1.2. Ví dụ 2

![](/assets/img/![](/assets/img/2025-09-08-22-23-13.png).png)

Câu: “The price of an apple an panana is $10”

- Biến số
  - a = giá táo.
  - b = giá chuối.
- Phương trình
$$a + b = 10$$

> Một phương trình, hai ẩn → vô số nghiệm.

Ví dụ nghiệm:
- a = 6, b = 4
- a =  3.5, b = 6.5
- a = 10, b = 0
- ... any pair summing to 10.

Bổ sung câu để đủ nghiệm duy nhất: "The apple costs twice the banana.” → a = 2b

$$
\begin{cases}
a + b = 10\\[4pt]
a - 2b = 0
\end{cases}
\quad\Rightarrow\quad
\text{solve: } 2b + b = 10 \Rightarrow b=\tfrac{10}{3},\; a=\tfrac{20}{3}.
$$

---
### 1.3. Quizzes

#### Quizz 1

You go two days in a row and collect this information:
- Day 1: You bought an apple and a banana and they cost $10.
- Day 2: You bought an apple and two bananas and they cost $12.

>**Question**: How much does each fruit cost?

**Solution**:
- Day 1: You bought an apple and a banana and they cost $10.

![](/assets/img/2025-09-09-10-47-21.png)

![](/assets/img/2025-09-09-10-50-23.png)

- Day 2: You bought an apple and two bananas and they cost $12.

![](/assets/img/2025-09-09-10-49-56.png)

![](/assets/img/2025-09-09-10-50-55.png)

- Solution: An apple costs $8, a banana costs $2. 

---
#### Quiz 2

You go two days in a row and collect this information:
- Day 1: You bought an apple and a banana and they cost $10.
- Day 2: You bought two apples and two bananas and they cost $20.
> **Question**: How much does each fruit cost? 

- Day 1: You bought an apple and a banana and they cost $10.

![](/assets/img/2025-09-09-10-47-21.png)

- Day 2: You bought two apples and two bananas and they cost $20.

![](/assets/img/2025-09-09-10-47-30.png)

---

![](/assets/img/2025-09-09-10-47-47.png)

---
#### Quiz 3

You go two days in a row and collect this information:
- Day 1: You bought an apple and a banana and they cost $10.
- Day 2: You bought two apples and two bananas and they cost $24.
> **Question**: How much does each fruit cost?

![](/assets/img/2025-09-09-10-53-13.png)

![](/assets/img/2025-09-09-10-53-19.png)

### 2.2. Systems of equations

![](/assets/img/2025-09-09-10-53-30.png)

---

![](/assets/img/2025-09-09-10-53-43.png)

---

![](/assets/img/2025-09-09-10-53-53.png)

---

![](/assets/img/2025-09-09-10-54-00.png)

--- 

### 2.3. A linear equation

![](/assets/img/2025-09-09-10-54-26.png)

### 2.4. From Linear equation to line

![](/assets/img/2025-09-09-10-56-15.png)

---

![](/assets/img/2025-09-09-10-56-51.png)

---

![](/assets/img/2025-09-09-10-57-10.png)

---

![](/assets/img/2025-09-09-10-57-19.png)

---

![](/assets/img/2025-09-09-10-57-29.png)

---

![](/assets/img/2025-09-09-10-57-37.png)

---

![](/assets/img/2025-09-09-10-57-46.png)

---

### 2.5 Systems of equations as lines

![](/assets/img/2025-09-09-10-58-53.png)

---

### A geometric notion of singularity

![](/assets/img/2025-09-09-10-59-07.png)

---

![](/assets/img/2025-09-09-10-59-22.png)

---

### 2.6 Singular vs nonsingular matrices

![](/assets/img/2025-09-09-10-59-48.png)

### Linear dependence and independence 

![](/assets/img/2025-09-09-11-01-50.png)

### The determinant - Linear dependence between rows 

![](/assets/img/2025-09-09-11-02-07.png)

### The determinant

![](/assets/img/2025-09-09-11-02-22.png)

![](/assets/img/2025-09-09-11-02-38.png)

### Determinant and singularity 

![](/assets/img/2025-09-09-11-03-10.png)

<!-- 

## II. Machine Learning Motivation

-  Many machine learning tasks involve solving systems of equations to find model parameters.

- Example: fitting a line 
$$ y = ax + b $$ 
through data points leads to a system of two-variable equations.

- Understanding systems of equations helps us see how ML models find optimal solutions.

## 2. Systems of Sentences

- Analogy: a sentence is like an equation — it encodes constraints.
- A system of sentences combines multiple constraints, which may be consistent or contradictory.
- Example:
- Sentence 1: “x is even.”
- Sentence 2: “x is odd.” → contradiction (no solution).
- Sentence 2: “x is divisible by 4.” → consistent system (solutions exist).

## 3. Systems of Equations

General form for two equations in two variables:

$$
a_1x + b_1y = c_1
$$
$$
a_2x + b_2y = c_2
$$

Possible outcomes:
- One unique solution.
- No solution (inconsistent).
- Infinitely many solutions (dependent).

## 4. Systems of Equations as Lines

- Each linear equation in two variables represents a line in the plane.
- The solution to the system is the intersection point of the lines.
- Cases:
  - Two lines intersect → one unique solution.
  - Two lines are parallel → no solution.
  - Two lines coincide → infinitely many solutions.

## 5. A Geometric Notion of Singularity

- “Singular” means a collapse of geometric structure.
- For two variables:

  - If the row (or column) vectors are independent, they span an area ≠ 0 → unique solution.

  - If dependent, they collapse into a single line → no solution or infinitely many solutions.

## 6. Singular vs Nonsingular Matrices

- Coefficient matrix:

$$
  A = \begin{bmatrix} a_1 & b_1 \\ a_2 & b_2 \end{bmatrix}
$$

- **Nonsingular** (invertible): 

$$
  A^{-1} \ \text{exists} \ \Rightarrow \ \text{unique solution}
$$


- **Singular** (non-invertible): no inverse → either no solution or infinitely many solutions.

## 7. Linear Dependence and Independence

- If two row (or column) vectors are linearly independent, they span a nonzero area → unique solution.
- If dependent, they lie on the same line → collapse of dimension.
- Examples:

$$(2,4) and (1,2) → dependent.$$  
$$(2,3) and (1,2) → independent. $$

## 8. The Determinant
- For a matrix (2x2):  

$$
  \det(A) = a_1b_2 - a_2b_1
$$  

- Interpretation:  
  - Absolute value is the area of the parallelogram formed by the vectors.  
  - If det(A) = 0 → vectors are dependent → singular matrix.  
  - If det(A) ≠ 0 → independent → unique solution.   -->
