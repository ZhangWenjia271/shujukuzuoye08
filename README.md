# shujukuzuoye08

## 题目一

考虑一个用于记录学生（student）在不同课程段（section）在不同考试中取得成绩（grade）的数据库，其中课程段属于某个课程（course）。

1. 绘制E-R图，只用二元联系。确保能够表示一个学生在不同考试中获得的成绩，且一个课程段可能有多次考试。（提示：使用多值属性） 

### 需要用到的实体集：
1. **student** (学生)
   - 属性：ID（主码），name, tot_cred

2. **course** (课程)
   - 属性：course_id（主码），title, credits

3. **section** (课程段)
   - 属性：(course_id, sec_id, semester, year) 复合主码

### 需要用到的联系集：
1. **takes** (选课/考试记录)
   - 关联：student与section
   - 属性：grade（成绩）
   - 说明：表示学生在特定课程段获得的成绩
   - 基数：多对多（一个学生可以选多个section，一个section有多个学生）

### E-R图关键点：
- section作为course的弱实体（通过course_id关联）
- takes联系包含grade属性，记录每次考试/课程的成绩
- 一个section可以有多个学生成绩记录（通过takes联系）


### E-R图绘制：
 ![image](https://github.com/user-attachments/assets/b8d7ebbe-05d1-4830-8c48-3a40cc33b0f7)


### 设计说明：
1. 该设计直接使用原数据库模式中的takes关系来记录成绩，符合题目要求
2. 通过takes关系的复合主码可以唯一标识一个学生在特定课程段的成绩
3. 不需要额外创建exam实体，因为原模式中成绩直接关联到section
4. 满足题目所有要求：
   - 记录学生在不同课程段的成绩
   - 一个课程段可以有多个学生成绩记录
   - 使用二元联系（student与section通过takes关联）

##
2. 写出上面E-R图的关系模式（要求注明主码）。

### 关系模式：
1. **student**(ID, name, tot_cred)
   - 主码：ID

2. **course**(course_id, title, credits)
   - 主码：course_id

3. **section**(course_id, sec_id, semester, year)
   - 主码：(course_id, sec_id, semester, year)
   - 外码：course_id 参照 course(course_id)

4. **takes**(ID, course_id, sec_id, semester, year, grade)
   - 主码：(ID, course_id, sec_id, semester, year)
   - 外码1：ID 参照 student(ID)
   - 外码2：(course_id, sec_id, semester, year) 参照 section

## 题目二

如果一个关系模式中只有两个属性，证明该关系模式必定属于BCNF。

### BCNF定义
一个关系模式R属于BCNF，当且仅当对于每一个形如X→Y的函数依赖：
1. X→Y是平凡的函数依赖，或者
2. X是R的一个超码

### 两种情况分析

设关系模式R(A,B)

**情况1：没有非平凡函数依赖**
- 只有平凡依赖：A→A，B→B，AB→A，AB→B等
- 这些都不违反BCNF
- 自动满足BCNF

**情况2：存在非平凡函数依赖**
可能的非平凡函数依赖有：
1. A→B
2. B→A

对于非平凡依赖：

**子情况2.1：只有A→B**
- 检查A是否是超码：
  - A的闭包A⁺ = {A,B} = R的所有属性
  - 因此A是超码
  - 满足BCNF

**子情况2.2：只有B→A**
- 同理，B⁺ = {A,B}
- B是超码
- 满足BCNF

**子情况2.3：同时有A→B和B→A**
- 此时A和B互为决定因素
- 两者的闭包都是{A,B}
- 二者都是超码
- 满足BCNF

**结论：**
在二元关系中，任何可能的函数依赖情况都不会违反BCNF，因此必定属于BCNF。

## 题目三

考虑关系模式`r(A, B, C, D, E)`，有如下函数依赖：

- A → BC
- BC → E
- CD → AB

请给出一个满足BCNF的分解，并说明你的分解符合BCNF。

### 已知
关系模式R(A,B,C,D,E)
函数依赖：
1. A → BC
2. BC → E
3. CD → AB

### 解答
**步骤1：找出所有候选码**
计算所有属性的闭包：
- A⁺ = {A,B,C,E}，（因为A→BC，BC→E），缺少 D，则A 不是候选码。
- CD⁺ = {A,B,C,D,E}（因为CD→AB，A→BC，BC→E）
- 没有比其更小的集合或其他组合（如 BD、AD 等）能决定所有属性
- 因此候选码是CD

**步骤2：检查违反BCNF的函数依赖**

检查每个函数依赖的决定因素是否是超码：

1. A → BC：
   - A⁺ = {A,B,C,E} ≠ {A,B,C,D,E}
   - A不是超码 → 违反BCNF

2. BC → E：
   - BC⁺ = {B,C,E} ≠ {A,B,C,D,E}
   - BC不是超码 → 违反BCNF

3. CD → AB：
   - CD⁺ = {A,B,C,D,E}
   - CD是超码 → 满足BCNF

**步骤3：分解过程**

选择第一个违反BCNF的依赖A→BC进行分解：

1. 分解为：
   - R1(A,B,C,E) - 包含A和A决定的属性
   - R2(A,D) - 剩余属性

检查R1(A,B,C,E)的函数依赖：
- 由于存在：
  - A → BC
  - BC → E
- 检查A→BC：
  - A⁺ = {A,B,C,E} = R1
  - 在R1中A是超码 → 满足BCNF
- 检查BC→E：
  - BC⁺ = {B,C,E} ≠ {A,B,C,E}
  - BC在R1中不是超码 → 违反BCNF

因此需要进一步分解R1：

选择BC→E进行分解：

2. 分解R1为：
   - R11(B,C,E) - 包含BC和E
   - R12(A,B,C) - 剩余属性

检查R11(B,C,E)：
- 只有BC→E
- BC⁺ = {B,C,E} = R11
- BC是超码 → 满足BCNF

检查R12(A,B,C)：
- 只有A→BC
- A⁺ = {A,B,C} = R12
- A是超码 → 满足BCNF

检查R2(A,D)：
- 没有函数依赖
- 所有属性构成超码 → 满足BCNF

**步骤4：验证分解**

验证无损连接：
- 分解过程遵循无损连接分解算法
- 每次分解都包含公共属性

验证依赖保持：
- A→BC保留在R12中
- BC→E保留在R11中
- CD→AB可以通过R12(A,B,C)和R2(A,D)连接后推导出

**最终BCNF分解：**
1. R1(A,B,C)：
   - 主码：A
   - 函数依赖：A→BC

2. R2(B,C,E)：
   - 主码：BC
   - 函数依赖：BC→E

3. R3(A,D)：
   - 主码：AD
   - 无函数依赖

4. R4(C,D)：
   - 需要添加以保持CD→AB的推导能力
   - 但实际上CD→AB可以通过R1和R3的自然连接推导

更优化的分解：
1. R1(A,B,C) - A→BC
2. R2(B,C,E) - BC→E
3. R3(C,D,A) - CD→A（因为CD→AB，但B已在R1中）

这样可以通过连接R1和R3得到CD→AB。
