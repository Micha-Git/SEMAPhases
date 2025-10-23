# Writing Go Doc Comments
This best practice defines the code documentation style for the implementation of a microservice in Golang [Go-The]. It bases on the official Go doc page [Go-Doc]. The description of the Go types is inspired from [Dig-How].

All public Go functions, interfaces, methods, and structs should be documented. In the context of Go, a public function/interface/method/struct is visible/accessible from the outside of the package in which it is declared/defined. As a reminder, a function/interface/method/struct is public if and only if its name starts with a capital letter. If the name starts with a lower case letter, the visibility/accessibility is private, which means that the function/interface/method/struct is only visible/accessible within the package in which it is declared/defined. If necessary, private Go functions, interfaces, methods, and structs can be documented, too.

## Basic Comment Structure
Each comment should have the following basic structure:
```
// <Name>
//
// <Further description>
```
A heading (indicated by `#`) can be included, too:
```
// <Name>
//
// # <Heading>
//
// <Further description>
```
`<Name>` is the exact name of the function/interface/method/struct. `<Further description>` depends on the object that is commented. If the basic comment structure above is used, it can be referenced using `[<package>.<type>]`, where `<type>` is the name of a function/interface/method/struct, and `<package>` is the name of the Go package in which `<type>` is declared/defined. If a Go type is declared/defined in package `x`, and the documentation shall be referenced within the same package `x`, only `<type>` is used; i.e., no package name and no square braces are needed. Additionally, the documentation reference of functions and methods does not require the round braces after the name of the function/method.

## Commenting Functions
> A function is a reusable section of code. It has a name, zero or more input and output parameters, and a (function) body. The number of input parameters can be variadic whereas the number of output parameters/return values should be restricted to 0, 1, or 2.

Functions should be commented according to the following structure. After the name, a short explanation of the function should follow. Then, the input parameters are listed chronologically and briefly explained. If there are no input parameters, `Parameters: None` is used. At last, the output parameters are briefly described. There should be one short description in the case of success and one description in the case of errors; i.e., all possible errors should be described. If there are no output parameters, `Returns: Nothing` is used. If the signature of the function is self-explanatory, the documentation should be as minimal as possible, or neglected.
```
// <Function name>
//
// <Summmary/Short description>
//
// Parameters:
//  - <First input parameter name> - <Short description>
//  - <Second input parameter name> - <Short description>
//  [...]
//  - <Last input parameter name> - <Short description>
//
// Returns:
//  - <Short description of the return value(s) after success>
//  - <Description of the possible error(s)>
func <Function name>(<Input parameters>) <Output parameters> {...}
```
Example:
```
// MapVPStatus
//
// Maps a request status from Verified ID (VID) to the corresponding [model.VPStatus].
//
// Parameters:
//   - vidRequestStatus - A request status from VID as a string
//
// Returns:
//   - The [model.VPStatus] that corresponds to the given request status from VID
//   - [model.UNSPECIFIED] if the given request status is invalid or unknown
func MapVPStatus(vidRequestStatus string) model.VPStatus {...}

```

## Commenting Interfaces
> An interface defines the behavior of a type by offering functions. Go structs can implement an interface by implementing all functions offered by that interface.

Interfaces should be commented using only one or two sentences. It is also possible to comment an interface using only a heading. All functions offered by the interface should be documented according to the Section [Commenting Functions](#commenting-functions). If a struct implements the interface, the documentation of the functions should be taken over. The documentation of the implementation of the function can be adapted, if necessary.
```
// <Interface name>
//
// <Description>
type <Interface name> interface {
	<Documentation of the first offered function>
	<First offered function>

	[...]
	
	<Documentation of the last offered function>
	<Last offered function>
}
```
Example:
```
// CarRepositoryInterface
//
// # The interface to the car repository which manages the persistent storage of cars
type CarRepositoryInterface interface {
	// GetCar
	//
	// Retrieves the car identified by the Vehicle Identification Number (VIN) given as a parameter.
	//
	// Parameters:
	//  - vin - The VIN of the car to retrieve
	//
	// Returns:
	//  - The car identified by the given VIN
	//  - An error if the given VIN is invalid
	//  - An error if there is no stored car with the given VIN
	GetCar(vin Vin) (Car, error)
}
```

## Commenting Methods
> Methods are special functions whose purpose is to operate on instances of some specific type, called a receiver.

The [documentation of functions](#commenting-functions) is the same for methods.

## Commenting Structs
> Structs collect different pieces of data together and organize them under different field names.

Depending on their use, structs should be described in detail. If necessary, their fields can be referenced or mentioned in their description. Each declared field should be documented, too.
```
// <Struct name>
//
// <Description>
type <Struct name> struct {
	<Documentation of the first field>
	<Name of the first field> <Data type of the first field>

	[...]

	<Documentation of the last field>
	<Name of the last field> <Data type of the last field>
}
```
Example:
```
// Car
//
// Represents a car with a Vehicle Identification Number (VIN), a brand, and a model.
type Car struct {
	// The Vehicle Identification Number (VIN) of the car
	Vin   Vin

	// The brand of the car
	Brand string

	// The model of the car
	Model string
}
```

---
**References**:
- [Go-The] [Go: The Go Programming Language](https://go.dev/)
- [Go-Doc] [Go: Go Doc Comments](https://tip.golang.org/doc/comment)
- [Dig-How] [DigitalOcean: How To Code in Go](https://www.digitalocean.com/community/tutorial-series/how-to-code-in-go)