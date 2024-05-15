# Delphi Language Overview

source: [Delphi Language Reference](https://docwiki.embarcadero.com/RADStudio/Athens/en/Delphi_Language_Reference)

## Types

### Ordinal Types

#### Functions on ordinal types

| Function | Parameter                                           | Return value                      |
| -------- | --------------------------------------------------- | --------------------------------- |
| `Ord`    | Ordinal expression                                  | Ordinality of expression's value  |
| `Pred`   | Ordinal expression                                  | Predecessor of expression's value |
| `Succ`   | Ordinal expression                                  | Successor of expression's value   |
| `High`   | Ordinal type identifier or variable of ordinal type | Highest value in type             |
| `Low`    | Ordinal type identifier or variable of ordinal type | Lowest value in type              |

#### Integer Types

| Type        | Alias    | Range                                     | Format          |
| ----------- | -------- | ----------------------------------------- | --------------- |
| `ShortInt`  | `Int8`   | -128..127                                 | Signed 8-bit    |
| `SmallInt`  | `Int16`  | -32768..32767                             | Signed 16-bit   |
| `FixedInt`  | `Int32`  | -2147483648..2147483647                   | Signed 32-bit   |
| `Integer`   | `Int32`  | -2147483648..2147483647                   | Signed 32-bit   |
|             | `Int64`  | -9223372036854775808..9223372036854775807 | Signed 64-bit   |
| `Byte`      | `UInt8`  | 0..255                                    | Unsigned 8-bit  |
| `Word`      | `UInt16` | 0..65535                                  | Unsigned 16-bit |
| `FixedUInt` | `UInt32` | 0..4294967295                             | Unsigned 32-bit |
| `Cardinal`  | `UInt32` | 0..4294967295                             | Unsigned 32-bit |
|             | `UInt64` | 0..18446744073709551615                   | Unsigned 64-bit |

#### Real Types

| Type       | Size in bytes      | Significant decimal digits |
| ---------- | ------------------ | -------------------------- |
| `Real48`   | 6                  | 11 - 12                    |
| `Single`   | 4                  | 7 - 8                      |
| `Double`   | 8                  | 15 - 16                    |
| `Real`     | 8                  | 15 - 16                    |
| `Extended` | platform dependent | platform dependent         |
| `Comp`     | 8                  | 10 - 20                    |
| `Currency` | 8                  | 10 - 20                    |

### String Types

| Type                            | Character Type | Maximum length (characters) | Memory required |
| ------------------------------- | -------------- | --------------------------- | --------------- |
| `ShortString`                   |                | 255                         | 2 - 256 bytes   |
| `AnsiString`                    | `AnsiChar`     | 2^31                        | 4 bytes - 2GB   |
| `UnicodeString`, alias `string` | `Char`         | 2^30                        | 4 bytes - 2GB   |
| `WideString`                    | `WideChar`     | 2^30                        | 4 bytes - 2GB   |

### Structured Types

#### Arrays

##### Static Arrays

```delphi
array[inexType1, ..., indexTypeN] of baseType;
```

```delphi
var MyArray: array[1..100] of Char;
```

##### Dynamic Arrays

```delphi
var MyFlexibleArray: array of Real;
begin
    SetLength(MyFlexibleArray, 20);
end;
```

```delphi
type TMyFlexibleArray = array of Integer;

begin
    MyFlexibleArray := TMyFlexibleArray.Create(1, 2, 3 {...});
end;
```

```delphi
procedure MyProc;
var
    A: array of Integer;
begin
    A := [1, 2, 3];
end;
```

```delphi
var
    A, B: array of Integer;
begin
    SetLength(A, 1);
    A[0] := 1;
    B := A;
    B[0] := 2;
    { A[0] equals 2 }
end;

// -------------------------
var
    A, B: array of Integer;
begin
    SetLength(A, 1);
    A[0] := 1;
    B := Copy(A);
    B[0] := 2;
    { A[0] <> B[0] }
end;
```

#### Records

```delphi
type recordTypeName = record
    fieldList1: type1;
        ...
        fieldListN: typeN;
    end;
```

```delphi
    type TDateRec = record
    Year: Integer;
        Month: (Jan, Feb, Mar, Apr, May, Jun, Jul, Aug, Sep, Oct, Nov, Dec);
        Day: 1..31;
    end;


    var Record1: TDateRec;
    begin
    Record1.Year := 2000;
        Record1.Month := Jan;
        Record1.Day := 1;
    end;
```

```delphi
    var S: record
    Name: string;
        Age: Integer;
    end;
```

#### File Types

```delphi
    type fileTypeName = file of type;
```

```delphi
    type PhoneEntry = record
    FirstName, LastName: string[20];
        PhoneNumber: string[20];
        Listed: Boolean;
    end;

    PhoneList = file of PhoneEntry;
```

```delphi
var
    MyFile: File of type;
    Data: array[1..50] of type;
    Filename: String;
begin
    Filename := 'MyExampleFile';
    AssignFile(MyFile, Filename);
    Reset(MyFile);
    for i := 1 to 50 do Read(MyFile, Data[i]);
    CloseFile(MyFile);
end;
```

```delphi
var
    MyFile: File of type;
    Data: array[1..50] of type;
    Filename: String;
begin
    Filename := 'MyExampleFile';
    AssignFile(MyFile, Filename);

    if FileExists(Filename) then
        Reset(MyFile)
    else
        Rewrite(MyFile);

    for i := 1 to 50 do Write(MyFile, Data[i]);
    CloseFile(MyFile);
end;
```

## Variables

Declaration:

```delphi
var identifierList: type;

// global variables only:
var identifier: type = constantExpression;
```

## Control Flow

### If Statements

```delphi
if expression then statement;
if expression then statement1 else statement2;
```

```delphi
if J <> o then
begin
    Result := I / J;
    Count := Count + 1;
end
else if Count = Last then
    Done := True
else
    Exit;
```

### Case Statements

```delphi
case selectorExpression of
    caseList1: statement1;
    ...
    caseListN: statementN;
end;

case selectorExpression of
    caseList1: statement1;
    ...
    caseListN: statementN;
else
    statements;
end;
```

```delphi
case I of
    1..5: Caption := 'Low';
    6..9: Caption := 'High';
    0, 10..99: Caption := 'Out of range';
else
    Caption := '';
end;


case MyColor of
    Red: X := 1;
    Green: X := 2;
    Blue: X = 3;
    Yellow, Orange, Black: X := 0;
end;
```

### Repeat Statements

```delphi
repeat statement1; ...; statementN; until expression;
```

```delphi
repeat
    K := I mod J;
    I := J;
    J := K;
until J = 0;
```

-   `expression` tested **after** each iteration

### While Statements

```delphi
while expression do statement;
```

```delphi
while I > 0 do
begin
    if Odd(I) then Z := Z * X;
    I := I div 2;
    X := Sqr(X);
end;
```

-   `expression` tested **before** each iteration

### For Statements

```delphi
for counter := initialValue to finalValue do statement;
for counter := initialValue downto finalValue do statement;
```

```delphi
for I := 2 to 63 do
    if Data[I] > Max then
        Max := Data[I];
```

-   value of `counter` is `undefined` after for statement

### Iteration over Containers

```delphi
for Element in ArrayExpression do statement;
for Element in StringExpression do statement;
for Element in SetExpression do statement;
for Element in CollectionExpression do statement;
for Element in Record do statement;
```

-   iteration variable cannot be modified within the loop

```delphi
var
    IArray: array[0..9] of Integer = (1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
    I: Integer;
begin
    for I in IArray do
    begin
    { Do something with I... }
    end;
end.
```

```delphi
var
    C: Char;
    S1, S2: String;
begin
    S1 := 'Example String';
    S2 := '';

    for C in S1 do
        S2 := S2 + C;
end.
```

## Procedures and Functions

### Declaration

```delphi
procedure procedureName(parameterList);
    localDeclarations;
begin
    statements;
end;
```

```delphi
function functionName(parameterList): returnType;
    localDeclarations;
begin
    statements
end;
```

```delphi
function WF: Integer;
begin
    WF := 17;
end;

{ same as: }

function WF: Integer;
begin
    Result := 17;
end;
```

### Overloading Procedures and Functions

```delphi
function Divide(X, Y: Real): Real; overload;
begin
    Result := X/Y;
end

function Divide(X, Y: Integer): Integer; overload;
begin
    Result := X div Y;
end;
```

### Nested Routines

```delphi
procedure DoSomething(S: string);
var
    X, Y: Integer;

    procedure NestedProc(S: string);
    begin
    ...
    end;

    begin
    ...
    NestedProc(S);
    ...
end;
```

### Value and Variable Parameters

```delphi
function DoubleByValue(X: Integer): Integer;   // X is a value parameter
begin
    X := X * 2; // mutates local X
    Result := X;
end;

function DoubleByRef(var X: Integer): Integer;  // X is a variable parameter
begin
    X := X * 2; // mutates X in outer / caller scope
    Result := X;
end;
```

### Array Parameters

```delphi
{ invalid: }
procedure Sort(A: array[1..10] of Integer);  // syntax error

{ valid: }
type TDigits = array[1..10] of Integer;
procedure Sort(A: TDigits);
```

### Default Parameters

```delphi
procedure FillArray(A: array of Integer; Value: Integer = 0);

{ calls }
FillArray(MyArray);
FillArray(MyArray, 0);
```
