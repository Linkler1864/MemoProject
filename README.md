**Архитектура**
![C4-Code (Class Diagram) drawio](https://github.com/user-attachments/assets/7c2af892-639e-4679-a95c-6a6f2b7ee6c4)
![C4-Components drawio](https://github.com/user-attachments/assets/1bf82017-a2cb-4d66-8163-9c213f8c940f)
![C4-Containers drawio](https://github.com/user-attachments/assets/3d9df6e6-7783-42a0-8b88-50f2751e48e5)
![DB ERD-Страница 1 drawio](https://github.com/user-attachments/assets/8abed3f8-9e45-4fae-a0fe-70b643d1d05c)
![java](https://github.com/user-attachments/assets/dd556998-3258-4b1c-9524-d8b52a6f2da9)
![UML drawio](https://github.com/user-attachments/assets/f21d5af0-ce40-43f3-8d18-f1e59ffe1f3f)
![UML-Страница 2 drawio](https://github.com/user-attachments/assets/b70c59b5-afc4-4b8a-b081-a0b7882f0641)

**Пользовательский интерфейс**

*_User flow_ диаграммы*
![user-flow-Page-1 drawio](https://github.com/user-attachments/assets/f75caa83-62ac-43e5-9c00-e016dc14abd2)
![user-flow-Страница 2 drawio](https://github.com/user-attachments/assets/6a506edb-f391-40aa-be30-036b47bcb66d)

*_Примеры экранов_*
![image](https://github.com/user-attachments/assets/f146d1a1-bff5-4872-b6d3-a85da6863074)
![image](https://github.com/user-attachments/assets/0d707e3a-b455-40b3-a3fa-ef4fa47b9afe)
![image](https://github.com/user-attachments/assets/13daec45-a762-456b-8b91-54489ba0ac29)
![image](https://github.com/user-attachments/assets/db35801d-6632-43e5-b065-ac64a72d44b7)
![image](https://github.com/user-attachments/assets/aa976adf-a8bf-42f2-834c-f5af98549c32)
![image](https://github.com/user-attachments/assets/0ff15340-dab1-4583-bf1d-86323b8d1abd)

**Безопасность**
![image](https://github.com/user-attachments/assets/fc06509f-e7d4-429b-b5e8-b34c94c53850)
![image](https://github.com/user-attachments/assets/24223fc9-93d1-4afc-ac94-8c05257c7470)
![image](https://github.com/user-attachments/assets/53c9b8b6-7469-49e3-b790-4f0ad0c1ef30)

**Документация**
В файле api-docs

![image](https://github.com/user-attachments/assets/5be2b293-7e7d-48e1-82fa-4e013d317375)
![image](https://github.com/user-attachments/assets/61993da6-802c-4744-80ca-41480ce51372)

**Оценка качества кода**
![image](https://github.com/user-attachments/assets/60e91fc0-3158-4303-b9de-bd2534203eb1)
![image](https://github.com/user-attachments/assets/b62fc06a-182c-4bec-904a-838d0ab9235d)
![image](https://github.com/user-attachments/assets/35971511-9ed5-4bec-8dc3-2c470694c5e6)
Процент закомментированности кода: 6.4%

**Тестирование**
![image](https://github.com/user-attachments/assets/17bc9d81-248c-46f6-bd11-5f621a425a6c)
Пример unit теста:
@Test
public void testFindAll() {
    
    Department department1 = new Department("HR");
    Department department2 = new Department("Finance");
    when(departmentRepository.findAll(Sort.by(Sort.Direction.DESC, "id")))
            .thenReturn(Arrays.asList(department1, department2));

    
    var departments = departmentService.findAll();

    
    assertNotNull(departments);
    assertEquals(2, departments.size());
    assertEquals("HR", departments.get(0).getName());
}

Пример интеграционного теста с использованием Spring Boot и Junit:
@Test
@WithMockUser(roles = "ADMIN")
public void testUpdate() throws Exception {
    when(toConverter.convert(any(DepartmentDto.class))).thenReturn(department);
    when(departmentService.update(eq("1"), any(Department.class))).thenReturn(department);
    when(toDtoConverter.convert(any(Department.class))).thenReturn(departmentDto);

    mockMvc.perform(put("/departments/1")
                    .contentType(MediaType.APPLICATION_JSON)
                    .content(new ObjectMapper().writeValueAsString(departmentDto)))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$.success").value(true))
            .andExpect(jsonPath("$.statusCode").value(StatusCode.SUCCESS))
            .andExpect(jsonPath("$.data.id").value("1"))
            .andExpect(jsonPath("$.data.name").value("HR"))
            .andExpect(jsonPath("$.data.description").value("Human Resources"));

    verify(departmentService, times(1)).update(eq("1"), any(Department.class));
}
