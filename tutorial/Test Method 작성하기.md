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

위와 같이 테스트 메서드를 생성합니다. (AAA Pattern)[http://wiki.c2.com/?ArrangeActAssert] 에 따라 작성합니다.

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

Assert 클래스를 이용하여 특정 값을 검증할 수 있습니다. 위 코드는 숫자 값을 받아, 6자리 숫자로 된 문자열을 만드는 코드를 검증하는 테스트 코드입니다.

[Assert 클래스 참고](https://msdn.microsoft.com/ko-kr/library/microsoft.visualstudio.testtools.unittesting.assert.aspx)
