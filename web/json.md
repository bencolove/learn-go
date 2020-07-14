# json
`encoding/json`
* encode
    * `json.Marshal(T)`
* decode 
    * `json.Unmarshal([]byte(string), &result)`

Any JSON data could be decoded into structured or unstructured (`map[string]interface{}`) type.

```go
package main

import (
	"encoding/json"
	"fmt"
	"strings"
)

type Result struct {
	String string 
	IntegratNumber int8
	FloatNumber float64
	// explicitly use pointer to accept <null> otherwise zero value of the type will be used
	OptionalInt *int8 `json:"optional"`
	// same as optional
	MayMissing *int8
	// ignore when empty
	StringArray []string `json:"array,omitempty"`
	AnyArray []ResultSubObject
	Object ResultSubObject
}

type ResultSubObject struct {
	Name string
	Age uint8
}

type Bird struct {
	Species     string `json:"birdType"`
	Description string `json:"what it does"`
}

func main() {
	fmt.Println("Hello, playground")

	
	// instantiate zero value as nil
	structuredJson()

	unstructuredJson()
}

func structuredJson() {
	fmt.Printf("%s -- decode json to structured data -- %s\n", strings.Repeat("=", 10), strings.Repeat("=", 10))
	
	jsonString := `{
		"string": "string",
		"IntegratNumber": 32,
		"FloatNumber": 64.0,
		"Optional": null,
	
		"array": ["a", "b", "c"],
		"AnyArray": [
			{
				"name": "roger",
				"age": 18
			},{
				"name": "jim",
				"age": 8
			}
		],
		"Object": {
			"name": "roger",
			"age": 18
		} 
	}`
	
	var result Result
	error := json.Unmarshal([]byte(jsonString), &result)
	cvt_rst := "success"
	if error != nil {
		cvt_rst = "fail"
	}
	fmt.Printf("decode sucess :%s\n %+v \n", cvt_rst, result)
	
	fmt.Printf("%s -- encode json to structured data -- %s\n", strings.Repeat("=", 10), strings.Repeat("=", 10))
	obj := &Result{
		String: "string",
		IntegratNumber: 16,
	FloatNumber:8.0,
	// explicitly use pointer to accept <null> otherwise zero value of the type will be used
	OptionalInt: nil,
	// same as optional
	MayMissing: nil,
	// ignore when empty `json:"array,omitempty"`
	StringArray: []string{},
	AnyArray:[]ResultSubObject{
		ResultSubObject{
			Name: "roger",
			Age: 28,
		},
		ResultSubObject{"jim", 18},
	},
	Object: ResultSubObject{"jim", 18},
	}
	
	data, err := json.Marshal(obj)
	if err != nil {
		fmt.Println(err)
	} else {
		fmt.Printf("encoded data typed: %T, value: %s\n", data, string(data))
	}
	
}

func unstructuredJson() {
	fmt.Printf("%s -- decode json to unstructured data -- %s\n", strings.Repeat("=", 10), strings.Repeat("=", 10))

	birdJson := `{
		"type_assert":{
			"string":"blar",
			"integrat_number": 123,
			"float_number": 45.6,
			"null":null,
			"bool":true,
			"array":[1,2,3],
			"object":{
				"key":"value"
			}	
		}
	}`
	// unmarshalled result container with zero value of nil
	var result map[string]interface{}
	
	// convert
	json.Unmarshal([]byte(birdJson), &result)

	
	// convert a JSON object value as `map` by type assertion
	if type_asserts, ok := result["type_assert"].(map[string]interface{}); ok {
		// sure thing
		for key, value := range type_asserts {
			fmt.Printf("JSON value:<%s> typed: %T\n", key, value)
		}
		
		//fmt.Print("test_type ")
		//switch test_value.(type) {
		//	case string: fmt.Println("is string")
		//	case float64: fmt.Println("is number")
		//	default: fmt.Println("is something i donnot know")
		//}
		
		// retrieve an optional value by switch.assert
		type_name := "unknown"
		type_value := type_asserts["null"] 
		switch type_value.(type) {
			case string: type_name = "string"
			case float64: type_name = "number"
			case bool: type_name = "bool"
			case []interface{}: type_name = "array"
			case map[string]interface{}: type_name = "object"
			case nil: type_name = "null"
		}
		fmt.Printf("assert type of JSON value:[%v] is %s\n", type_value, type_name) 
	} else {
		fmt.Println("test_type not existed")
	}
	
	fmt.Printf("%s -- encode unstructured data to json -- %s\n", strings.Repeat("=", 10), strings.Repeat("=", 10))
	
	birdData := map[string]interface{}{
		"birdSounds": map[string]string{
			"pigeon": "coo",
			"eagle":  "squak",
		},
		"total birds": 2,
	}

	
	// JSON encoding is done the same way as before	
	data, _ := json.Marshal(birdData)
	fmt.Println(string(data))
}
```  
OUTPUT:  
``` shell
========== -- decode json to structured data -- ==========
decode sucess :success
 {String:string IntegratNumber:32 FloatNumber:64 OptionalInt:<nil> MayMissing:<nil> StringArray:[a b c] AnyArray:[{Name:roger Age:18} {Name:jim Age:8}] Object:{Name:roger Age:18}} 
========== -- encode json to structured data -- ==========
encoded data typed: []uint8, value: {"String":"string","IntegratNumber":16,"FloatNumber":8,"optional":null,"MayMissing":null,"AnyArray":[{"Name":"roger","Age":28},{"Name":"jim","Age":18}],"Object":{"Name":"jim","Age":18}}
========== -- decode json to unstructured data -- ==========
JSON value:<integrat_number> typed: float64
JSON value:<float_number> typed: float64
JSON value:<null> typed: <nil>
JSON value:<bool> typed: bool
JSON value:<array> typed: []interface {}
JSON value:<object> typed: map[string]interface {}
JSON value:<string> typed: string
assert type of JSON value:[<nil>] is null
========== -- encode unstructured data to json -- ==========
{"birdSounds":{"eagle":"squak","pigeon":"coo"},"total birds":2}
```