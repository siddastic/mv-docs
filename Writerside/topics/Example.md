# Example

```Typescript
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';

class Input extends StatelessWidget {
  final String? Function(String? value)? validator;
  final TextEditingController? controller;
  final String? hintText;
  final IconData? prefixIcon;
  final Widget? prefix;
  final int? maxLength;
  final TextInputType? keyboardType;
  final bool isSafeInput;
  final bool? autofocus;
  final Function(String)? onChange;
  const Input({
    this.validator,
    this.controller,
    this.hintText,
    this.prefixIcon,
    this.prefix,
    this.maxLength,
    this.keyboardType,
    this.isSafeInput = false,
    this.autofocus,
    this.onChange,
    super.key,
  });

  @override
  Widget build(BuildContext context) {
    return TextFormField(
      validator: validator,
      controller: controller,
      keyboardType: keyboardType,
      obscureText: isSafeInput,
      onChanged: onChange,
      maxLength: maxLength,
      maxLengthEnforcement: maxLength != null
          ? MaxLengthEnforcement.enforced
          : MaxLengthEnforcement.none,
      autofocus: autofocus ?? false,
      decoration: InputDecoration(
        isDense: true,
        hintText: hintText,
        prefixIcon: prefixIcon != null
            ? Icon(
                prefixIcon,
                color: Colors.black.withOpacity(.5),
              )
            : null,
        prefix: prefix,
        border: OutlineInputBorder(
          borderRadius: BorderRadius.circular(12),
          borderSide: BorderSide(
            color: Colors.black.withOpacity(.5),
            width: .5,
          ),
        ),
        enabledBorder: OutlineInputBorder(
          borderRadius: BorderRadius.circular(12),
          borderSide: BorderSide(
            color: Colors.black.withOpacity(.5),
            width: .5,
          ),
        ),
        focusedBorder: OutlineInputBorder(
          borderRadius: BorderRadius.circular(12),
          borderSide: const BorderSide(
            color: Colors.black,
            width: .75,
          ),
        ),
      ),
    );
  }
}
```
{default-state="collapsed" collapsible="true" collapsed-title="input.dart (Helper widget)"}

```Typescript
import 'package:flutter/material.dart';

import 'package:master_validator/master_validator.dart';
import 'package:master_validator_example/widgets/primary_button.dart';

import 'widgets/input.dart';
import 'widgets/space.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatefulWidget {
  const MyApp({super.key});

  @override
  State<MyApp> createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
  @override
  void initState() {
    super.initState();
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      theme: ThemeData(
        primaryColor: const Color(0xff262626),
        appBarTheme: const AppBarTheme(
          centerTitle: true,
          color: Color(0xff262626),
          elevation: 0,
          titleTextStyle: TextStyle(
            color: Colors.white,
          ),
          iconTheme: IconThemeData(
            color: Colors.white,
          ),
        ),
        elevatedButtonTheme: ElevatedButtonThemeData(
          style: ElevatedButton.styleFrom(
            backgroundColor: const Color(0xff262b40),
            foregroundColor: const Color(0xff181b28),
          ),
        ),
        useMaterial3: true,
      ),
      debugShowCheckedModeBanner: false,
      home: const FormValidationExample(),
    );
  }
}

enum SnackbarMessageType { Success, Error }

class FormValidationExample extends StatefulWidget {
  const FormValidationExample({super.key});

  @override
  State<FormValidationExample> createState() => _FormValidationExampleState();
}

class _FormValidationExampleState extends State<FormValidationExample> {
  final _formKey = GlobalKey<FormState>();

  void showSnackbar(String message, SnackbarMessageType type) {
    ScaffoldMessenger.of(context).hideCurrentSnackBar();
    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(
        content: Text(message, style: const TextStyle(color: Colors.white)),
        backgroundColor:
            type == SnackbarMessageType.Success ? Colors.green : Colors.red,
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Master Validator Example'),
      ),
      body: ListView(
        padding: const EdgeInsets.symmetric(horizontal: 16, vertical: 20),
        children: [
          const Space(20),
          const Text("Validators.Email (Required)"),
          Space.def,
          Form(
            autovalidateMode: AutovalidateMode.onUserInteraction,
            key: _formKey,
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Input(
                  validator: Validators.Required(next: Validators.Email()),
                  prefixIcon: Icons.alternate_email_rounded,
                  hintText: "Email",
                ),
                const Space(20),
                const Text("Validators.Number (Optional)"),
                Space.def,
                Input(
                  validator: Validators.Number(),
                  prefixIcon: Icons.numbers,
                  hintText: "Phone Number",
                ),
                const Space(20),
                const Text(
                    "Validators.Minlength + Validators.Maxlength (Required)"),
                Space.def,
                Input(
                  validator: Validators.Required(
                    errorMessage: "Username cannot be empty",
                    next: Validators.MinLength(
                      errorMessage: "Username must be at least 3 characters",
                      length: 3,
                      next: Validators.MaxLength(
                        length: 8,
                        errorMessage: "Username must be at most 8 characters",
                      ),
                    ),
                  ),
                  prefixIcon: Icons.person,
                  hintText: "Username",
                ),
                const Space(20),
                const Text("Validators.LengthBetween"),
                Space.def,
                Input(
                  validator: Validators.LengthBetween(3, 10),
                  prefixIcon: Icons.key,
                  hintText: "Password",
                ),
                const Space(20),
                const Text("Validators.URL (Optional)"),
                Space.def,
                Input(
                  validator: Validators.Url(),
                  prefixIcon: Icons.link,
                  hintText: "URL",
                ),
                const Space(20),
                const Text("Validators.Regex (Optional)"),
                Space.def,
                Input(
                  validator: Validators.Regex(
                    pattern: r'^#?([0-9a-fA-F]{3}|[0-9a-fA-F]{6})$',
                    errorMessage: "Invalid Hex Color Code",
                  ),
                  prefixIcon: Icons.link,
                  hintText: "Hex Color Code ex - #000000",
                ),
                const Space(20),
                const Text("Validators.Equals (Optional)"),
                Space.def,
                Input(
                  validator: Validators.Equals(
                    value: '123456',
                    errorMessage:
                        "Password matches with previous password, enter a new password",
                  ),
                  prefixIcon: Icons.key,
                  hintText: "New Password (123456)",
                ),
                const Space(20),
                const Text("Validators.FileName (Optional)"),
                Space.def,
                Input(
                  validator: Validators.FileName(),
                  prefixIcon: Icons.file_present,
                  hintText: "File Name",
                ),
                const Space(20),
                const Text("Validators.FileSize"),
                Space.def,
                Input(
                  validator: Validators.DirectoryName(),
                  prefixIcon: Icons.folder,
                  hintText: "Directory Name (Optional)",
                ),
              ],
            ),
          ),
          Space.def,
          PrimaryButton(
            label: "Validate",
            onclick: () {
              // validate form
              if (_formKey.currentState!.validate()) {
                showSnackbar("Form is valid", SnackbarMessageType.Success);
              } else {
                showSnackbar("Form is invalid", SnackbarMessageType.Error);
              }
            },
          ),
        ],
      ),
    );
  }
}
```
{collapsible="true" collapsed-title="main.dart" default-state="collapsed"}