# Memory Reducing Techniques

Let's see how different types of numeric datatypes store value -123
| Type    | Internal storage | Usage | Bytes |
|-------------|----------|----------|----------|
| Zone Decimal| X’F1F2D3’| Display  | 3 Bytes  |
| Packed Decimal| X’12D3’| Comp-3   | 2 Bytes  | 
| Binary   | X’0001 0010 1111 0011’  | Comp, Comp-4, Comp-5 | 2 Bytes|

## Packed Decimal / Comp-3
- Comp-3 is one of the efficient ways to store numeric data and reduce memory.
- It stores 2 digits in one byte and the memory is calculated based on the picture clause length if even `n/2+1` if odd `(n+1)/2`
- The extra one byte we are adding is for the rightmost byte that stores the sign value and even though you didn’t provide it, it’ll store a zone number in it.
- Value= 123(3bytes) | Packed Decimal = X’12F3’ (2 bytes)
- **Advantages:**
    - Whenever you perform arithmetic operations on a numeric field, Cobol **converts** the **Zone** decimal to **Packed** decimal or Binary to process it.
    - But comp-3 stores **directly packed** decimal digits into the memory. So it **reduces** the **CPU power** while performing **calculations**.
- **Disadvantages:**
    - You **can’t display** it directly.
    - Uses **more storage** **than Binary** format.
 
## Binary / Comp / Comp-4 / Comp-5
- Comp is also an efficient way to store the integer data to reduce the memory.
- Comp stores the numeric data in **binary** format.
- The memory is calculated based on the pic length.
    - if the pic length is `01 - 04 = 16 bits = 2 Bytes = Half Word`
    - if the pic length is `05 - 09 = 32 bits = 4 Bytes = Full Word`
    - If the pic length is `10 -18 = 64 bits = 8 Bytes = Double word`
- **Advantages:**
    - It stores **more integers** than **comp-3**
- **Disadvantages:**
    - We may **lose accuracy** while performing arithmetic operations on **fraction values**.
    - Cuz **comp-3** calculates fractions with a **negative power of 10** but **comp** stores as a **negative power of 2.**
    - Need to be **converted** into **packed decimal** and into **Zone decimal** to **display** comp fields.

## Which is better Comp or Comp-3
**Comp**

- Organizations related to financials like banking and insurance, need to be very accurate in calculations.
- Comp stores the data in binary format which has a negative power of 2 and while performing calculations on fraction data it loses accuracy and needs to be converted into packed decimal or zone decimal to display them. This makes comp data less accurate in fraction calculation and increases the CPU time due to the conversion process.

**Comp-3**

- it directly stores the data in packed decimal format making it easier to perform calculations with good accuracy and making CPU time less.

## Explain the slag byte concept
Cobol stores the sign value of any numeric data (Zone decimal, Comp, comp-3…) in the rightmost byte without taking any extra memory, this concept is called a slag byte. 

C =  +ve sign

D = -ve sign

F = Unsigned

## Difference b/w renames & redefines
| RENAMES | REDEFINES |
| --- | --- |
| It’s used to regroup the variables or rename a single variable. Creates an alias for an existing data item | It reuses the already-existing memory from previous variables for upcoming variables |
| It’s not a memory-reducing technique | It’s a memory-reducing technique |
| Level no: 66 | Not a Level no|
Applicable for only elementary data items | Applicable for group, elementary, and independent data items. |
| No need for a PIC clause | The PIC clause is mandatory |
| Limited to data division only | We can use it in any sections file, working-storage, etc. |
