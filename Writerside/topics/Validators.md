# Validators

## Usage Syntax

```Typescript
TextFormField(
    validator: Validators.<Validator-Name>(
        errorMessage : "<When this validator results to error>"
        next : <Next-Chained Validator>,
    ),
),
```

Master Validator provides the following methods

Validators.Required()
: To mark a field required

Validators.Email()
: Email address validation

Validators.Number()
: For field working with numeric inputs (ex. Phone number, Quantity) with optional integerOnly allowNegative allowDecimal parameters.

Validators.LengthBetween()
: Returns true only if the string satisfies the length boundaries.

Validators.Minlength()
: Works like LengthBetween but only for the lower bound.

Validators.Maxlength()
: Works like LengthBetween but only for the upper bound.

Validators.Url()
: URL Validation

Validators.Regex()
: Custom validation through a regular expression

Validators.Equals()
: The value in text-field should exactly by equal to the value provided

Validators.FileName()
: Validates file names (Useful when working with I/O operations), also has an argument to customize extensions which you wish to accept.

Validators.DirectoryName()
: Validates Directory names (Useful when working with I/O operations)

<tip title="Validator Chaining">
All the validators can be chained, i.e. every validator contains a `next` argument in which you can pass-on another validator, for instance

A password validator which is required and should have character-length between 8-15 would look like

```Typescript
TextFormField(
    validator: Validators.Required(
        errorMessage : "Enter Password"
        next : Validators.LengthBetween(8, 15,
            errorMessage : "Password should atleast be 8 characters long and 15 max",
        ),
    ),
),
```
</tip>