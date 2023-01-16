# Parameterized tests

```python 
#@pytest.mark.parametrize(
#    "arg, expected", [([0, 4, 2, 8], 428), ([1, 2], 12), ([3, 5, 1], 351)]
#)
#
#pass arg, and expected into pytest test function to to run a function with the "args" and compared to "expected output, for example:

@pytest.mark.parametrize(
    "arg, expected", [([0, 4, 2, 8], 428), ([1, 2], 12), ([3, 5, 1], 351)]
)
def test_valid_input(arg, expected):
    assert list_to_decimal(arg) == expected
    ```