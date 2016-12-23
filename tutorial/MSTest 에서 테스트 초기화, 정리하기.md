# 테스트 코드 Attributes

아래 코드는 TPON의 예시입니다.

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

#### TestMethod

- 테스트 method드임을 지정하는 attribute

#### TestClass

- 테스트 class임을 지정하는 attribute

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
