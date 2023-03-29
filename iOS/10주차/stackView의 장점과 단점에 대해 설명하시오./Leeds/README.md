# stackView의 장점과 단점에 대해 설명하시오

### **Stack View?**

- AutoLayout을 적용해 내부에 배치된 View들을 열 또는 행에 배치해주는 인터페이스

![Screen Shot 2022-02-14 at 11.59.52 PM.png](https://user-images.githubusercontent.com/122095401/228577272-39e89aaf-6526-4caf-829b-88d5524c08c3.png)

<br>

### **언제 사용할까?**

Layout을 잡을 때는 무조건 Stack View를 쓰고, 

Stack View로 해결할 수 없을때만 직접 조건(Constraints)을 설정한다.

<br>

### **장점**

- 다양한 View를 단위로 그룹화 하여 관리할 수 있기 때문에 유지보수성 ↑
- 다양한 View와 Layout 조합을 지원하므로, 복잡한 UI 구성시 유용
- 레이아웃이 동적으로 조정되기 때문에 화면의 크기가 달라져도 View의 배치가 자동으로 조정됨

### **단점**

- 런타임 시 동적으로 변경되는 View의 개수에 따라 성능 저하
- View의 제약 조건이 충돌하거나 예상대로 동작하지 않는 경우<br>
→ View가 충분한 크기를 갖지 못하거나 제약 조건이 많아서 Stack View의 다른 View와 충돌

<br>

---

<br>

StackView 배치 방법 정리글

[https://medium.com/compass-true-north/uistackview-and-auto-layout-b16fd2c026c0](https://medium.com/compass-true-north/uistackview-and-auto-layout-b16fd2c026c0)
