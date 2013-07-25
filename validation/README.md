validation
==============

validation is a form validation for a data validation and error collecting using Go.

## Installation and tests

Install:

	go get github.com/astaxie/beego/validation

Test:

	go test github.com/astaxie/beego/validation

## Example

Direct Use:

	import (
		"github.com/astaxie/beego/validation"
		"log"
	)

	type User struct {
		Name string
		Age int
	}

	func main() {
		u := User{"man", 40}
		valid := validation.Validation{}
		valid.Required(u.Name, "name")
		valid.MaxSize(u.Name, 15, "nameMax")
		valid.Range(u.Age, 0, 140, "age")
		if valid.HasErrors {
			// validation does not pass
			// print invalid message
			for _, err := range valid.Errors {
				log.Println(err.Key, err.Message)
			}
		}
		// or use like this
		if v := valid.Max(u.Age, 140); !v.Ok {
			log.Println(v.Error.Key, v.Error.Message)
		}
	}

Struct Tag Use:

	import (
		"github.com/astaxie/beego/validation"
	)

	// validation function follow with "valid" tag
	// functions divide with ";"
	// parameters in parentheses "()" and divide with ","
	type user struct {
		Id   int
		Name string `valid:"Required"`
		Age  int    `valid:"Required;Range(1, 140)"`
	}

	func main() {
		valid := Validation{}
		u := user{Name: "test", Age: 40}
		b, err := valid.Valid(u)
		if err != nil {
			// handle error
		}
		if !b {
			// validation does not pass
			// blabla...
		}
	}

Struct Tag Functions:

	Required
	Min(min int)
	Max(max int)
	Range(min, max int)
	MinSize(min int)
	MaxSize(max int)
	Length(length int)
	Alpha
	Numeric
	AlphaNumeric
	Match(regexp string)		// does not support yet
	NoMatch(regexp string)	// does not support yet
	AlphaDash
	Email
	IP
	Base64


## LICENSE

BSD License http://creativecommons.org/licenses/BSD/