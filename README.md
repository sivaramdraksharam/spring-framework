# spring-framework
Learn spring and sprng boot

Dependency Injection:
A bean can become a dependency to another bean.
Example: An Employee class has Address class type reference.
class Address
{
    private int houseNumber;
    private String landMark;
    //no-args consgtructor
    //getters and setters
    //override toString()
  }

class Employee
{
    private int empId;
    private String empName;
    private Address address;
  //no-args consgtructor

  //constructor injection - we are passing Address object as parameter
    public Employee(Address address)
    {
      this.address = address;
    }
    //getters and setters
    //override toString()
  }

