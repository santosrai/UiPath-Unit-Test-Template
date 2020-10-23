# UiPath-unit-test-template
This is for unit test for uipath project

Thanks to Stefan Krsmanović who have created this template
[uipath-testing_framework](https://devpost.com/software/uipath-testing_framework)

Quick guide:

* Install custom activity packages from **Gallery** tab of [Package manager](https://studio.uipath.com/docs/managing-activities-packages#section-managing-packages) or from the [UiPath Go](https://connect.uipath.com/profile/stefan-krsmanovic/components)
	* _Custom.Activities.VariableComparer.x.x.x.nupkg_
	* _TestingFrameworkUi.x.x.x.nupkg_
* Using **Unit Test** template (Tests_Repository\UnitTestTemplate\MethodName_StateUnderTest_ExpectedBehavior.xaml) make Unit Tests.
	* **Unit tests** must implement **Assert Unit Test** custom activity to be eligible for running in **Framework**.
* Place **Unit Tests** into the **Tests_Repository** folder. Feel free to make new subfolders inside of Tests_Repository folder, or its subfolders.
	* When **RunAllTests.xaml** is started it will open a GUI that will allow you to select which tests to run from **Tests_Repository** folder or its subfolders. GUI is also used to show test results.
* To integrate **TestingFramework** into another project: 
	* Copy whole folder to the target project root directory. 
	* Delete **project.json** file from **TestingFramework** folder (if present).
	* Add **Custom.Activities.VariableComparer.x.x.x.nupkg** and **TestingFrameworkUi.x.x.x.nupkg** packages as dependencies to the main projects **project.json** file. 

## Custom.Activities.VariableComparer.x.x.x.nupkg package ##

_Custom.Activities.VariableComparer.x.x.x.nupkg_ package is custom developed code for comparing variables, and is a core part of the Framework:

* It contains custom **Assert Unit Test** activity that must be used when making **Unit Tests**.
* **Assert Unit Test** activity is used as a container for writing **Assert methods** ([imported](https://studio.uipath.com/docs/importing-new-namespaces) from _CustomActivities.VariableComparer_ namespace) which are used to evaluate unit test outputs.
	* Assert.AreEqual(expected,actual) - contains method overloads that are used to check if the compared variables contain the exact same values.
	* Assert.AreNotEqual(expected,actual) - contains method overloads that are used to check if the compared variables doesn't contain the exact same values.
	* Assert.AreSame(expected,actual) - contains method overloads that are used to check if the compared variables reference the same object in memory.
	* Assert.AreNotSame(expected,actual) - contains method overloads that are used to check if the compared variables doesn't reference the same object in memory.
	* Assert.Contains(item, target) - contains method overloads that are used to check if the item is contained inside of the target object.
	* Assert.NotContains(expected,actual) - contains method overloads that are used to check if the item is not contained inside of the target object.
	* Assert.IsNull(actual) - checks if variable is referencing null object
	* Assert.IsNotNull(actual) - checks if variable is not referencing null object
* **Assert methods** throw custom exceptions that are used by the **Framework** to log test results and give important information in case of a test failiure.
	* AssertException is main exception thrown by Assert methods.
	* AssertNullException is exception thrown when Assert.IsNull or Assert.IsNotNull method returns false.
	* CustomAssertException is generic exception that is thrown when random boolean expression that returns false is passed to Assert Unit Test activity

