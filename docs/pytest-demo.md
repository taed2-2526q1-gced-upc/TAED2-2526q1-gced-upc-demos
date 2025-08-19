# Pytest demo <!-- omit in toc -->
In this demo we will see the main features of [Pytest](https://docs.pytest.org/en/latest/) to test a simple machine
learning project.

## Contents <!-- omit in toc -->
- [Install Pytest](#install-pytest)
- [Create the test file](#create-the-test-file)
- [Run the tests](#run-the-tests)
- [Measure test coverage (Optional)](#measure-test-coverage-optional)

## Install Pytest
The first step is to install `pytest`. To do this, we can run the
following command:
```bash
uv add --group dev pytest
```

## Create the test file
We can now create a test file called [`test_model.py`](../tests/test_model.py) in the `tests` directory. In this file,
we will create a test to check that our model meets our expectations.

To do this, we will create a fixture called `pipe` that will load the model from the `models` directory and another 
called `test_ds` that will load the test dataset. Then, we will create a test function called `test_model_accuracy` that
will use the previous fixtures to load the model and check that the model accuracy on the test set is above a certain threshold.

Next, we create a second test called `test_model_predictions` to see if the model still works when changing one word in
the input. In this case, we want to test the model for multiple inputs. To do this, we will use the `pytest.mark.parametrize`
decorator to parametrize the test function. This decorator receives the names of the parameters and a list of values.

Finally, we will add the following lines to the [`pyproject.toml`](../pyproject.toml) file to configure `pytest`:
```toml
[tool.pytest.ini_options]
pythonpath = "."
testpaths = "tests"
```

In detail:
- `pythonpath` specifies the path to the source code;
- `testpaths` specifies the directory where the tests are located;

## Run the tests
We can now run the tests using the following command:
```bash
pytest
```

## Measure test coverage (Optional)
When the source code grows it can be difficult to know what is not being tested. For such cases, we can use the `pytest-cov` package to measure the test coverage. This package will generate a coverage report that shows which parts of the code are not being tested.

To install it we can run the following command:
```bash
uv add --group dev pytest-cov
```

Then, we can add the following to our `pyproject.toml` file:

```toml
[tool.coverage.run]
omit = ["src/data/validate_data.py", "src/data/gx_context_configuration.py", "src/modeling/train.py"]

[tool.pytest.ini_options]
...
addopts = "--junitxml=out/tests-report.xml --cov=src --cov-report=html:reports/coverage"
```

In detail:
- `omit` specifies the files that should be omitted from the coverage report. In this case, we omit some scripts that only
call functions that have been defined elsewhere.

- `addopts` specifies options to pass to `pytest`. In this case, we are specifying the path to the test results file
(`out/tests-report.xml`), which can be read by continuous integration tools, the path to the module we want to measure
the coverage (`src`), and the format (HTML) and directory where the coverage report should be saved (`reports/coverage`).
