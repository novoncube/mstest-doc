# 테스트 코드 초기화, 정리하기

테스트 코드 실행 전에, 시작 전후로 해야할 작업이 있을 수 있습니다. 초기화나 disconnect 같은 작업입니다. 아래의 2가지의 Attribute를 사용하면 됩니다.
- TestInitialize : TestClass가 실행할 때, 초기화 코드로 지정
- TestCleanup : TestMethod가 실행 후 종료한 뒤에 실행하는 코드

코드 예시는 아래와 같습니다. TPON의 예시입니다.

```cs

    [TestClass]
    public class NodeManagerHelper_Test
    {
        [TestInitialize]
        public void TestInit()
        {
            ServiceLocator.SetLocatorProvider(() => SimpleIoc.Default);
            SimpleIoc.Default.Register<IAlarmSeverityMethods, DefaultAlarmSeverityMethods>();
            SimpleIoc.Default.Register<NodeManager>();
            SimpleIoc.Default.Register<INodeRevisionServiceAction, DefaultNodeRevisionServiceAction>();
            SimpleIoc.Default.Register<INodeRevisionService, NodeRevisionService>(true);
        }

        [TestCleanup]
        public void Cleanup()
        {
            SimpleIoc.Default.Reset();
        }

        [TestMethod]
        public void NewGroup2()
        {
            // Arrange
            var dict = new Dictionary<string, object>
            {
                {"id", (long)new Random().Next()},
                {"pa", (long)new Random().Next()},
                {"lbl", Guid.NewGuid().ToString()},
                {"alm", (long?)new Random().Next()}
            };

            var json = JsonConvert.SerializeObject(dict);
            var jsonToken = JToken.Parse(json);

            var alm = AlarmSeverityHelper.AlarmStatusFromMetisAlarmState(((long?)dict["alm"]).Value);

            // Act
            var result = NodeManagerHelper.NewGroup2(jsonToken);

            // Assert
            Assert.AreEqual(dict["id"], result.Key);
            Assert.AreEqual(dict["pa"], result.ParentKey);
            Assert.AreEqual(dict["lbl"], result.Name);
            Assert.AreEqual(alm, result.AlarmStatus);
            Assert.AreEqual(NodeItemType.Group, result.ItemType);
        }

    }

```

### Test Attribute 정리

Test Attibute는 여러 케이스를 염두하여 만들어져 있습니다. 아래와 같이 정리합니다.

#### TestMethod

- 테스트용 메서드에 지정

#### TestClass

- 테스트용 클래스에 지정

#### ClassInitialize

- 해당 class에서 1회만 실행

#### ClassCleanup

- 해당 class에서 1회만 실행

#### TestInitialize

- TestMethod가 실행할 때 마다 실행

#### TestCleanUp

- TestMethod가 실행할 때 마다 실행

#### AssemblyInitialize

- 해당 테스트(어셈블리)에서 1회만 실행

#### AssemblyCleanUp

- 해당 테스트(어셈블리)에서 1회만 실행