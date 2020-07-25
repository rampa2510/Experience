# Architecture
1) Operations can be perfromed on same data types
2) While sending string through api calls we can convert string to bytes(utf8) implementation or rune (utf32) implementation to reduce data size.
3) When appedning to a slice how go completes the process is 
   1) If the length of new slice is less than double its capacity of previous slice then the capacity of new slice is doubled from the previous one and elements are appended
   2) If the length of new slice is more than double the capacity of previous slice it equates the capacity to the length of new element.
   3) If the data type increase like int32 it equates the length to be 25% more than the length of new array
4) Make func returns a pointer
5) Slices are actually refrences to actual memory blocks so they are called by refrences
6) Array are call by value 
7) If the first letter of var is CAPITAL the varaible will be exposed at global level
8) If the slice you want to append to is very big then what we can do is use the make func to create a slice with the cap we want and then append to it
9) To use validation for fields in structs in go we can use tags and reflect package to get those tags eg
```golang

type Employee struct {
	address string `required max:"100"`
	name    string
	age     int
}

func main() {
	t := reflect.TypeOf(Employee{})
	field, i := t.FieldByName("address")
	fmt.Println(field.Tag, i) //required max:"100" true
}
```
10) In golang when you assign a struct to a variabke and then you change the newly created structs properties it will not affect the old struct properties
11) Go uses composition rather than inheritance. It works on embedding. Example -
```golang
type animal struct {
	species   string
	ageOfSpan int
}

type bird struct {
	animal
	name string
}

func main() {
	b := bird{}
	b.species = "lol"
	b.ageOfSpan = 18
	b.name = "ram"
	fmt.Println(b.species) // {{lol 18} ram}
}

```
12) In golang the switch statements dont have fallthrough condition so if one case is true without explicit mentioning break go will break from the switch statements but if we want a fallthrough condition we can code the ```fallthrough``` keyword in the case statement
13) We can use label in golang - if we have a nested for loop and we want to break from both the for loops we can use label
```golang
func main() {
Loop:
	for i := 0; i < 3; i++ {
		for j := 0; j < 3; j++ {
			if j > 1 {
				break Loop
			}
		}
		fmt.Println("ll")
	}
}
```
This will never print ll as it will never reach that blog
14) One of the main use of defer is to code the creation and end of a resource together so that we do not forget to close the resource example
```golang

func main() {
	res, err := http.Get("https://www.lipsum.com/")
	defer res.Body.Close()
	if err != nil {
		log.Fatal(err)
	}
	robots, err := ioutil.ReadAll(res.Body)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Println(string(robots))

}

```
as we can see here we are creating the res variable and closing it but due to the functionalities of defer the res variable will be erased after main