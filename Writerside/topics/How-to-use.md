# Getting Started

Master validator is meant to be used when working with forms generally. The library provides a set of pre-written validator functions which you can give as a value to the `validator` argument of your text form field widget 

Import <shortcut>master_validator</shortcut> package

<code-block lang="javascript">
import 'package:master_validator/master_validator.dart';
</code-block>

## Usage

### Basic Required Field Validation

Use Validators.Required() to make a field required.

<code-block lang="typescript">
TextFormField(
    validator: Validators.Required(
        errorMessage : "This field cannot be left empty",
    )
),
</code-block>

Email Validation :

<code-block lang="typescript">
TextFormField(
    validator: Validators.Required(
        next : Validators.Email(),
    ),
),
</code-block>

<tip title="Note">
A Detailed list of all the available validators can be found <a href="Validators.md">here</a>
</tip>

### Using Multiple Validators (Chaining Validators)

<tip>
While Chaining, Order MATTERS! 
</tip>

All validators take a next argument in which you can specify another validator (a custom function, or another predefined validator) like :

<code-block lang="typescript">
TextFormField(
    validator: Validators.Required(
        next : Validators.Email(
            next : Validators.Maxlength(50)
        ),
    ),
),
</code-block>

> **Important!**
>
> By default, All Validators except Validators.Required() will not throw an error if the data is empty/null, this means if you defined a validator like :
> <code-block lang="typescript">
> TextFormField(
>  validator: Validators.Email(),
> ),
> </code-block>
> It means the field acts as an optional field but if something is entered it must qualify as an email, to change this behaviour, and to make the field required, use Validators.Required() before any other validator, for example -
> <code-block lang="typescript">
> TextFormField(
>   validator: Validators.Required(
>       next : Validators.Url(),
>   ),
> ),
> </code-block>

{style="note"}

## Finally

Finally, to validate your form, just call validate method from your form's key :

<code-block lang="typescript">
// supply the Form widget with the key
Form(
    key : _formKey,
    child : Column(),
)
</code-block>
<code-block lang="typescript">
// validate form inside some function
if (_formKey.currentState!.validate()) {
    showSnackbar("Form is valid");
} else {
    showSnackbar("Form is invalid");
}
</code-block>

### Extra

Master Validator by default also adds up the following extensions to String class :
* isEmail
* isInteger
* isDouble
* isValidDirectoryName
* isValidURL
* hasLengthBetween(min,max)
All of these returns a boolean value which can be used as :

```Typescript
String email = "someone@org.com";

if(email.isEmail){
    print("Valid Email");
}
```

For detailed usage, check <a href="Example.md">example</a>