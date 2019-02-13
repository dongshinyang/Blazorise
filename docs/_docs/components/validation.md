---
title: "Validation"
permalink: /docs/components/validation/
excerpt: "Learn how to use automatic and manual validation for input components."
toc: true
toc_label: "Guide"
---

Validation component is used to provide simple form validation for Blazorise input components. The basic structure for validation component is:

- `<Validations>` optional container for manual validation
  - `<Validation>` input container
    - `<Feedback>` messages placeholder
      - `<ValidationSuccess>` success message
      - `<ValidationError>` error message

## Basic validation

For the most part you will need to use just the `<Validation>` component along with `<ValidationSuccess>` and `<ValidationError>`. By default every validation will run automatically when input value changes. You must set the `Validate` event handler where you can define the validation rules and return the validation result.

### Example

Here you can see the basic example for automatic validation and custom function for checking the email.

```html
<Validation Validate="@ValidateEmail">
    <TextEdit Placeholder="Enter email">
        <Feedback>
            <ValidationSuccess>Email is good</ValidationSuccess>
            <ValidationError>Enter valid email!</ValidationError>
        </Feedback>
    </TextEdit>
</Validation>
@functions{
    void ValidateEmail( ValidateEventArgs e )
    {
        var email = Convert.ToString( e.Value );

        e.Status = string.IsNullOrEmpty( email ) ? ValidationStatus.None :
            email.Contains( "@" ) ? ValidationStatus.Success : ValidationStatus.Error;
    }
}
```

The same structure is for all **Edit** components(check, radio, select, etc). Note that for some components there are some special rules when defining the validation structure. For example for **CheckEdit** you must use `ChildContent` tag along with the `<Feedback>` tag. This is a limitation in Blazor, hopefully it will be fixed in the future.

```html
<Validation Validate="@ValidateCheck">
    <CheckEdit>
        <ChildContent>
            Check me out
        </ChildContent>
        <Feedback>
            <ValidationError>You must check me out!</ValidationError>
        </Feedback>
    </CheckEdit>
</Validation>
```

## Manual validation

Sometimes you don't want to do validation on every input change. In that case you use `<Validations>` component to group multiple validations and then run the validation manually.

### Example

In this example you can see how the `<Validations>` component is used to enclose multiple validation components and the `Mode` attribute is set to Manual. Validation is executed only when clicked on submit button.

```html
<Validations ref="validations" Mode="ValidationMode.Manual">
    <Validation Validate="@ValidateEmail">
        ...
    </Validation>
    <Validation Validate="@ValidatePassword">
        ...
    </Validation>
    <SimpleButton Color="Color.Primary" Clicked="@Submit">Submit</SimpleButton>
</Validation>
@functions{
    Validations validations;

    void Submit()
    {
        validations.Validate();
    }
}
```