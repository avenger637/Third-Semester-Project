# 15. Testing classes and plan

Suggested JUnit test classes:
- `LinkedListTest`
- `StackTest`
- `QueueTest`
- `HashTableTest`
- `HeapTest`
- `BinarySearchTreeTest`
- `GraphTest`
- `SortingAlgorithmsTest`
- `SearchingAlgorithmsTest`
- `SchedulingAlgorithmsTest`
- `BudgetAlgorithmsTest`
- `RegistrationAlgorithmsTest`
- `AuthenticationServiceTest`
- `AuthorizationServiceTest`
- `EventServiceTest`
- `RegistrationServiceTest`
- `FinanceServiceTest`

Typical test methods:
```java
@Test void testInsert();
@Test void testDelete();
@Test void testSearch();
@Test void testEmptyStructure();
@Test void testDuplicateValues();
@Test void testBoundaryConditions();
```

Algorithm tests:
```java
@Test void testMergeSort();
@Test void testQuickSort();
@Test void testBinarySearch();
@Test void testScheduleConflictDetection();
@Test void testBudgetLimit();
```

Include edge cases, complexity discussion, and tests proving one user cannot access another user's records.
