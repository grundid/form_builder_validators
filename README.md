# Form Builder Validators

Form Builder Validators set of validators for FlutterFormBuilder. Provides common validators and a way to make your own.

Also included is the `l10n` / `i18n` of error text messages to multiple languages.
___

[![Pub Version](https://img.shields.io/pub/v/form_builder_validators?logo=flutter&style=for-the-badge)](https://pub.dev/packages/form_builder_validators)
[![GitHub Workflow Status](https://img.shields.io/github/workflow/status/flutter-form-builder-ecosystem/form_builder_validators/Base?logo=github&style=for-the-badge)](https://github.com/flutter-form-builder-ecosystem/form_builder_validators/actions/workflows/base.yaml)
[![Codecov](https://img.shields.io/codecov/c/github/flutter-form-builder-ecosystem/form_builder_validators?logo=codecov&style=for-the-badge)](https://codecov.io/gh/flutter-form-builder-ecosystem/form_builder_validators/)
[![CodeFactor Grade](https://img.shields.io/codefactor/grade/github/flutter-form-builder-ecosystem/form_builder_validators?logo=codefactor&style=for-the-badge)](https://www.codefactor.io/repository/github/flutter-form-builder-ecosystem/form_builder_validators)


[![Buy me a coffee](https://www.buymeacoffee.com/assets/img/guidelines/download-assets-sm-1.svg)](https://www.buymeacoffee.com/danvick)
___

> ### Migrating from version 7 to 8
> To migrate from v7 to v8, remove `context` as a parameter to validator functions. For example, `FormBuilderValidators.required(context)` becomes `FormBuilderValidators.required()` without context passed to it.

### Example
```dart
import 'package:form_builder_validators/form_builder_validators.dart';

...

TextFormField(
  decoration: InputDecoration(labelText: 'Name'),
  autovalidateMode: AutovalidateMode.always,
  validator: FormBuilderValidators.required(),
),
TextFormField(
  decoration: InputDecoration(labelText: 'Age'),
  keyboardType: TextInputType.number,
  autovalidateMode: AutovalidateMode.always,
  validator: FormBuilderValidators.compose([
    FormBuilderValidators.numeric(errorText: 'La edad debe ser numérica.'),
    FormBuilderValidators.max(70),
    (val) {
      var number = int.tryParse(val ?? '');
      if (number != null) if (number < 0)
        return 'We cannot have a negative age';
      return null;
    }
  ]),
),
```

## Built-in Validators
This package comes with several most common `FormFieldValidator`s such as required, numeric, mail,
URL, min, max, minLength, maxLength, IP, credit card, etc., with default `errorText` messages.

Available built-in validators include:
* `FormBuilderValidators.creditCard()` - requires the field's value to be a valid credit card number.
* `FormBuilderValidators.date()` - requires the field's value to be a valid date string.
* `FormBuilderValidators.email()` - requires the field's value to be a valid email address.
* `FormBuilderValidators.equal()` - requires the field's value be equal to provided object.
* `FormBuilderValidators.integer()` - requires the field's value to be an integer.
* `FormBuilderValidators.ip()` - requires the field's value to be a valid IP address.
* `FormBuilderValidators.match()` - requires the field's value to match the provided regex pattern.
* `FormBuilderValidators.max()` - requires the field's value to be less than or equal to the provided number.
* `FormBuilderValidators.maxLength()` - requires the length of the field's value to be less than or equal to the provided maximum length.
* `FormBuilderValidators.min()` - requires the field's value to be greater than or equal to the provided number.
* `FormBuilderValidators.minLength()` - requires the length of the field's value to be greater than or equal to the provided minimum length.
* ``FormBuilderValidators.equalLength()`` - requires the length of the field's value to be equal to the provided minimum length.
* `FormBuilderValidators.numeric()` - requires the field's value to be a valid number.
* `FormBuilderValidators.required()` - requires the field have a non-empty value.
* ``FormBuilderValidators.url()`` - requires the field's value to be a valid url.

## Composing multiple validators
`FormBuilderValidators` class comes with a very useful static function named `compose()` which takes a list of `FormFieldValidator` functions. Composing allows you to create once and reuse validation rules across multiple fields, widgets, or apps.

On validation, each validator is run, and if any one validator returns a non-null value (i.e., a String), validation fails, and the `errorText` for the field is set as the returned string.

Example:
```dart
TextFormField(
  decoration: InputDecoration(labelText: 'Age'),
  keyboardType: TextInputType.number,
  autovalidateMode: AutovalidateMode.always,
  validator: FormBuilderValidators.compose([
    /// Makes this field required
    FormBuilderValidators.required(),

    /// Ensures the value entered is numeric - with custom error message
    FormBuilderValidators.numeric(errorText: 'La edad debe ser numérica.'),

    /// Sets a maximum value of 70
    FormBuilderValidators.max(70),

    /// Include your own custom `FormFieldValidator` function, if you want
    /// Ensures positive values only. We could also have used `FormBuilderValidators.min(0)` instead
    (val) {
      final number = int.tryParse(val);
      if (number == null) return null;
      if (number < 0) return 'We cannot have a negative age';
      return null;
    }
  ]),
),
```

## l10n
To allow for localization of default error messages within your app, add `FormBuilderLocalizations.delegate` in the list of your app's `localizationsDelegates`

```dart
  return MaterialApp(
      supportedLocales: [
        Locale('de'),
        Locale('en'),
        Locale('es'),
        Locale('fr'),
        Locale('it'),
        ...
      ],
      localizationsDelegates: [
        GlobalMaterialLocalizations.delegate,
        GlobalWidgetsLocalizations.delegate,
        FormBuilderLocalizations.delegate,
      ],
```
### Supported languages (default errorText messages)
- Arabic (ar)
- Bangla (bn)
- Catalan (ca)
- Chinese Simplified (zh_Hans)
- Chinese Traditional (zh_Hant)
- English (en)
- Estonian (et)
- Dutch (nl)
- Farsi/Persian (fa)
- French (fr)
- German (de)
- Hungarian (hu)
- Indonesian (id)
- Italian (it)
- Japanese (ja)
- Korean (ko)
- Lao (lo)
- Polish (pl)
- Portuguese (pt)
- Romanian (ro)
- Russian (ru)
- Slovak (sk)
- Slovenian (sl)
- Spanish (es)
- Swahili (sw)
- Ukrainian (uk)
- Turkish (tr)

And you can still add your custom error messages.

## Support
### Issues and PRs
Any support in reporting bugs, answering questions, or PRs is always appreciated.

We especially welcome efforts to internationalize/localize the package by translating the default validation `errorText` strings for built-in validation rules.

### Localizing messages

#### 1. Add ARB files
Create one ARB file inside the `lib/l10n` folder for each of the locales you need to add support. Name the files in the following way: `intl_<LOCALE_ISO_CODE>.arb`. For example: `intl_fr.arb` or `intl_fr_FR.arb`.

#### 2. Translate the error messages

Duplicate the contents of `intl_messages.arb` (or any other ARB file) into your newly created ARB file, then translate the error messages by overwriting the default messages.

#### 3. Run command
To generate boilerplate code for localization, run the generate command inside the package directory where `pubspec.yaml` file is located:

```
  flutter pub run intl_utils:generate
```

Running the command will automatically create/update files inside the `lib/localization` directory, including your newly added locale support.

#### 4. Update README
Remember to update README, adding the new language (and language code) under [Supported languages section](#supported-languages-default-errortext-messages) so that everyone knows your new language is now supported!

#### 5. Submit PR
Submit your PR and be of help to millions of developers all over the world!

### Coffee :-)
If this package was helpful to you in delivering your project, or you want to support this package, I would highly appreciate a cup of coffee ;-)

[![Buy me a coffee](https://www.buymeacoffee.com/assets/img/guidelines/download-assets-sm-1.svg)](https://www.buymeacoffee.com/danvick)

