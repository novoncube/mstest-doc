# Test Method 작성하기

## Test Method 생성

```cs
    [TestClass]
    public class TestSample
    {
        [TestMethod]
        public void Sample테스트()
        {
            // Arrange
            // Act
            // Assert
        }
    }
```

위와 같이 테스트 메서드를 생성합니다. [AAA Pattern](http://wiki.c2.com/?ArrangeActAssert]) 에 따라 작성합니다.

```cs
    [TestClass]
    public class TestSample
    {
        [TestMethod]
        public void Sample테스트()
        {
            // Arrange
            long num = 101;

            // Act
            var result = NodeManagerHelper.HexaLengthNumber(num);

            // Assert
            Assert.AreEqual(result, "000101");
        }
    }
```

위 코드는 6자리 숫자 문자열 생성에 대한 테스트 코드입니다. 

Assert class를 이용하여 테스트 결과 값을 검증할 수 있습니다. 

Assert class에 대한 자세한 설명은 아래의 링크를 참조하시길 바랍니다.

[Assert class](https://msdn.microsoft.com/ko-kr/library/microsoft.visualstudio.testtools.unittesting.assert.aspx)
