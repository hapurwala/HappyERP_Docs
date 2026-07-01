# Product Tax Calculation Logic

## Tax Structure

```text
Product Value Before Discount
            ↓
Discount Calculation
            ↓
Product Value After Discount
            ↓
Tax Calculation
            ↓
Net Product Value
```

---

# Definitions

## Product Value Before Discount

```text
product_value_before_discount
=
quantity × rate
```

Example:

```text
Quantity = 100
Rate = 50

100 × 50 = 5000
```

Result:

```text
product_value_before_discount = 5000
```

---

## Discount Calculation

### Fixed Discount

```text
discount_value
=
discount_rate
```

Example:

```text
Discount Type = Fixed
Discount Rate = 200

discount_value = 200
```

---

### Percentage Discount

```text
discount_value
=
product_value_before_discount
×
discount_rate
÷
100
```

Example:

```text
5000 × 10 ÷ 100

=
500
```

---

### Per Unit Discount

```text
discount_value
=
quantity × discount_rate
```

Example:

```text
100 × 2

=
200
```

---

## Product Value After Discount

```text
product_value_after_discount
=
product_value_before_discount
-
discount_value
```

Example:

```text
5000 - 500

=
4500
```

---

# Tax Calculation

## Taxable Value

For every tax row:

```text
products[].taxes[].taxable_value
=
product_value_after_discount
```

Example:

```text
4500
```

---

## Tax Amount

For each tax:

```text
products[].taxes[].tax_value
=
taxable_value
×
tax_rate_percent
÷
100
```

Example:

```text
Taxable Value = 4500
Tax Rate = 18%

4500 × 18 ÷ 100

=
810
```

Result:

```text
tax_value = 810
```

---

# Multiple Tax Calculation

Example:

```text
Taxable Value = 4500

CGST = 9%
SGST = 9%
```

### CGST

```text
4500 × 9 ÷ 100

=
405
```

### SGST

```text
4500 × 9 ÷ 100

=
405
```

Stored:

```json
[
  {
    "tax_name": "CGST",
    "tax_rate_percent": 9,
    "tax_value": 405
  },
  {
    "tax_name": "SGST",
    "tax_rate_percent": 9,
    "tax_value": 405
  }
]
```

---

# Total Tax Amount

```text
products[].tax_value
=
SUM(products[].taxes[].tax_value)
```

Example:

```text
405 + 405

=
810
```

Result:

```text
tax_value = 810
```

---

# Net Product Value

```text
products[].net_value
=
product_value_after_discount
+
tax_value
```

Example:

```text
4500 + 810

=
5310
```

Result:

```text
net_value = 5310
```

---

# Validation Rules

## Tax Rate

```text
tax_rate_percent >= 0
```

---

## Tax Value

```text
tax_value >= 0
```

---

## Taxable Value

```text
taxable_value >= 0
```

---

# Recalculation Triggers

Recalculate Taxes When:

* Product Changes
* Quantity Changes
* Rate Changes
* Discount Type Changes
* Discount Rate Changes
* Tax Selection Changes

---

# Formula Summary

```text
product_value_before_discount
=
quantity × rate

discount_value
=
based on discount type

product_value_after_discount
=
product_value_before_discount - discount_value

taxable_value
=
product_value_after_discount

tax_value
=
taxable_value × tax_rate_percent ÷ 100

products[].tax_value
=
SUM(all tax_value)

net_value
=
product_value_after_discount + tax_value
```
