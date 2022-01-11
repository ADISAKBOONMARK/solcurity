# The Solcurity Standard (TH)

Opinionated **security** and **code quality** standard for **Solidity smart contracts**. 

_Based off work by [BoringCrypto](https://github.com/sushiswap/bentobox/blob/master/documentation/checks.txt), [Mudit Gupta](https://www.youtube.com/watch?v=LLiJK_VeAvQ), [Runtime Verification](https://github.com/runtimeverification/verified-smart-contracts/wiki/List-of-Security-Vulnerabilities), and [ConsenSys Diligence](https://consensys.github.io/smart-contract-best-practices/known_attacks)_.

### General Review Approach:
- อ่าน Specs และ Whitepaper ของโครงการเพื่อทำความเข้าใจว่า Smartcontracts มีไว้ทำอะไรบ้าง
- สร้าง Model ของสิ่งที่คาดหวังว่า Smartcontracts จะมีหน้าตาเป็นอย่างไรก่อนที่จะ code
- ดู Smartcontracts ต่างๆ เพื่อทำความเข้าใจ Architecture ของโครงการ เครื่องมือแนะนำ [Surya](https://github.com/ConsenSys/surya) 
- ปรียบเทียบ Architecture กับ Model ของคุณ และหาสิ่งผิดปกติ
- สร้าง Model และ List ของความเสี่ยงเพื่อสร้างแบบจำลองสถานะการณ์การถูกโจมตี 
- ตรวจสอบส่วนที่สามารถเปลี่ยนแปลงค่าได้ เช่น Functions ดังต่อไปนี้ transfer, transferFrom, send, call, delegatecall, และ selfdestruct เป็นต้น เพื่อทำการตรวจสอบอีกครั้งว่าปลอดภัย
- ตรวจสอบ Interface ส่วนที่สามารถติดต่อกับ Smartcontracts ภายนอกได้ ให้แน่ใจว่าทำงานถูกต้อง เช่น ราคาต้องเพิ่มขึ้นเท่านั้น
- ทำการตรวจสอบ Smartcontracts อีกครั้งทีละบรรทัด
- ทบทวน Smartcontracts อีกครั้งจากมุมมองของทุกคนในรูปแบบความเสี่ยง
- ตรวจสอบว่า Test cases และ Code coverage ครอบคลุมทุกส่วนของ Code (หากพบว่าส่วนใดของ Code ไม่ถูกตรวจสอบ ให้ตรวจสอบ Code ในส่วนนี้เป็นพิเศษ)
- ใช้ Tools เช่น [Slither/Solhint](https://github.com/crytic/slither) ช่วยในการ Review และดูผลลัพธ์
- ดูโครงการที่เกี่ยวข้องเพื่อตรวจสอบปัญหาหรือการกำกับดูแลที่คล้ายกัน

## Variables

- `V1` - สามารถเป็น `internal` ได้หรือไม่ ?
- `V2` - สามารถเป็น `constant` ได้หรือไม่ ?
- `V3` - สามารถเป็น `immutable` ได้หรือไม่ ?
- `V4` - การตั้งค่าการมองเห็นของตัวแปร ([SWC-108](https://swcregistry.io/docs/SWC-108))
- `V5` - วัตถุประสงค์ของตัวแปรและข้อมูลสำคัญอื่น ๆ ถูกอธิบายไว้โดยใช้ [Natspec](https://docs.soliditylang.org/en/v0.8.10/natspec-format.html) หรือไม่ ?

- `V6` - สามารถ Optimize Code ได้หรือไม่ ([adjacent storage variable](https://docs.microsoft.com/en-us/cpp/c-language/storage-and-alignment-of-structures?view=msvc-170)) ?
- `V7` - สามารถ Packed ตัวแปรลงใน Struct ได้หรือไม่ ?
- `V8` - ใช้ตัวแปรได้เต็มประสิทธิภาพหรือไม่ ?
- `V9` - ถ้ามีตัวแปร Public Array และต้องการ Return ค่าทั้งหมด ให้ตรวจสอบว่ามี Function สำหรับ  Return the full array หรือไม่ ?
- `V10` - ใช้ `private` ในกรณีที่ต้องการป้องกันการเข้าถึงจาก Child contracts และใช้ `internal` ในกรณีที่ต้องการป้องกันการเข้าถึงจาก Contracts อื่น ยกเว้น Child contracts

## Structs

- `S1` - จำเป็นต้องเป็น Struct หรือไม่ ? หรือเป็นแค่ Variable ก็เพียงพอแล้ว
- `S2` - แต่ละ Fields ของ Struct เหมาะสมแล้วหรือไม่ ?
- `S3` - วัตถุประสงค์ของ Struct ถูกอธิบายไว้โดยใช้ [Natspec](https://docs.soliditylang.org/en/v0.8.10/natspec-format.html) หรือไม่ ?

## Functions

- `F1` - สามารถเป็น `external` ได้หรือไม่ ?
- `F2` - ควรเป็น `internal` หรือไม่ ?
- `F3` - ควรเป็น `payable` หรือไม่ ?
- `F4` - สามารถใช้งานร่วมกับ Function อื่น ๆ ได้ถูกต้องหรือไม่ ?
- `F5` - ตรวจสอบความถูกต้องของการ Validate all parameters ว่าทำได้ถูกต้องหรือไม่ แม้ว่า Function จะสามารถเรียกได้เฉพาะบุคคลที่เชื่อถึอได้ เช่น Owner, Admin, Operation เป็นต้น
- `F6` - Is the checks before effects pattern followed? (SWC-107)
- `F7` - Check for front-running possibilities, such as the approve function. (SWC-114)
- `F8` - Is insufficient gas griefing possible? (SWC-126)
- `F9` - Are the correct modifiers applied, such as `onlyOwner`/`requiresAuth`?
- `F10` - Are return values always assigned?
- `F11` - Write down and test invariants about state before a function can run correctly.
- `F12` - Write down and test invariants about the return or any changes to state after a function has run.
- `F13` - Take care when naming functions, because people will assume behavior based on the name.
- `F14` - If a function is intentionally unsafe (to save gas, etc), use an unwieldy name to draw attention to its risk.
- `F15` - Are all arguments, return values, side effects and other information documented using natspec?
- `F16` - If the function allows operating on another user in the system, do not assume `msg.sender` is the user being operated on.
- `F17` - If the function requires the contract be in an uninitialized state, check an explicit `initialized` variable. Do not use `owner == address(0)` or other similar checks as substitutes.
- `F18` - Only use `private` to intentionally prevent child contracts from calling the function, prefer `internal` for flexibility.
- `F19` - Use `virtual` if there are legitimate (and safe) instances where a child contract may wish to override the function's behavior.

## Modifiers

- `M1` - Are no storage updates made (except in a reentrancy lock)?
- `M2` - Are external calls avoided?
- `M3` - Is the purpose of the modifier and other important information documented using natspec?

## Code

- `C1` - Using SafeMath or 0.8 checked math? (SWC-101)
- `C2` - Are any storage slots read multiple times?
- `C3` - Are any unbounded loops/arrays used that can cause DoS? (SWC-128)
- `C4` - Use `block.timestamp` only for long intervals. (SWC-116)
- `C5` - Don't use block.number for elapsed time. (SWC-116)
- `C7` - Avoid delegatecall wherever possible, especially to external (even if trusted) contracts. (SWC-112)
- `C8` - Do not update the length of an array while iterating over it.
- `C9` - Don't use `blockhash()`, etc for randomness. (SWC-120)
- `C10` - Are signatures protected against replay with a nonce and `block.chainid` (SWC-121)
- `C11` - Ensure all signatures use EIP-712. (SWC-117 SWC-122)
- `C12` - Output of `abi.encodePacked()` shouldn't be hashed if using >2 dynamic types. Prefer using `abi.encode()` in general. (SWC-133)
- `C13` - Careful with assembly, don't use any arbitrary data. (SWC-127)
- `C14` - Don't assume a specific ETH balance. (SWC-132)
- `C15` - Avoid insufficient gas griefing. (SWC-126)
- `C16` - Private data isn't private. (SWC-136)
- `C17` - Updating a struct/array in memory won't modify it in storage.
- `C18` - Never shadow state variables. (SWC-119)
- `C19` - Do not mutate function parameters.
- `C20` - Is calculating a value on the fly cheaper than storing it?
- `C21` - Are all state variables read from the correct contract (master vs. clone)?
- `C22` - Are comparison operators used correctly (`>`, `<`, `>=`, `<=`), especially to prevent off-by-one errors?
- `C23` - Are logical operators used correctly (`==`, `!=`, `&&`, `||`, `!`), especially to prevent off-by-one errors?
- `C24` - Always multiply before dividing, unless the multiplication could overflow.
- `C25` - Are magic numbers replaced by a constant with an intuitive name?
- `C26` - If the recipient of ETH had a fallback function that reverted, could it cause DoS? (SWC-113)
- `C27` - Use SafeERC20 or check return values safely.
- `C28` - Don't use `msg.value` in a loop.
- `C29` - Don't use `msg.value` if recursive delegatecalls are possible (like if the contract inherits `Multicall`/`Batchable`).
- `C30` - Don't assume `msg.sender` is always a relevant user.
- `C31` - Don't use `assert()` unless for fuzzing or formal verification. (SWC-110)
- `C32` - Don't use `tx.origin` for authorization. (SWC-115)
- `C33` - Don't use `address.transfer()` or `address.send()`. Use `.call.value(...)("")` instead. (SWC-134)
- `C34` - When using low-level calls, ensure the contract exists before calling.
- `C35` - When calling a function with many parameters, use the named argument syntax.
- `C36` - Do not use assembly for create2. Prefer the modern salted contract creation syntax.
- `C37` - Do not use assembly to access chainid or contract code/size/hash. Prefer the modern Solidity syntax.
- `C38` - Use the `delete` keyword when setting a variable to a zero value (`0`, `false`, `""`, etc).
- `C39` - Comment the "why" as much as possible. 
- `C40` - Comment the "what" if using obscure syntax or writing unconventional code.
- `C41` - Comment explanations + example inputs/outputs next to complex and fixed point math.
- `C42` - Comment explanations wherever optimizations are done, along with an estimate of much gas they save.
- `C43` - Comment explanations wherever certain optimizations are purposely avoided, along with an estimate of much gas they would/wouldn't save if implemented.
- `C44` - Use `unchecked` blocks where overflow/underflow is impossible, or where an overflow/underflow is unrealistic on human timescales (counters, etc). Comment explanations wherever `unchecked` is used, along with an estimate of how much gas it saves (if relevant).
- `C45` - Do not depend on Solidity's arithmetic operator precedence rules. In addition to the use of parentheses to override default operator precedence, parentheses should also be used to emphasise it.
- `C46` - Expressions passed to logical/comparison operators (`&&`/`||`/`>=`/`==`/etc) should not have side-effects.
- `C47` - Wherever arithmetic operations are performed that could result in precision loss, ensure it benefits the right actors in the system, and document it with comments. 
- `C48` - Document the reason why a reentrancy lock is necessary whenever it's used with an inline or `@dev` natspec comment.
- `C49` - When fuzzing functions that only operate on specific numerical ranges use modulo to tighten the fuzzer's inputs (such as `x = x % 10000 + 1` to restrict from 1 to 10,000).
- `C50` - Use ternary expressions to simplify branching logic wherever possible.
- `C51` - When operating on more than one address, ask yourself what happens if they're the same.

## External Calls

- `X1` - Is an external contract call actually needed?
- `X2` - If there is an error, could it cause DoS? Like `balanceOf()` reverting. (SWC-113)
- `X3` - Would it be harmful if the call reentered into the current function?
- `X4` - Would it be harmful if the call reentered into another function?
- `X5` - Is the result checked and errors dealt with? (SWC-104)
- `X6` - What if it uses all the gas provided?

## Static Calls

- `S1` - Is an external contract call actually needed?
- `S2` - Is it actually marked as view in the interface?
- `S3` - If there is an error, could it cause DoS? Like `balanceOf()` reverting. (SWC-113)
- `S4` - If the call entered an infinite loop, could it cause DoS?

## Events

- `E1` - Should any fields be indexed?
- `E2` - Is the creator of the relevant action included as an indexed field?
- `E3` - Do not index dynamic types like strings or bytes.
- `E4` - Is when the event emitted and all fields documented using natspec?
- `E5` - Are all users/ids that are operated on in functions that emit the event stored as indexed fields?

## Contract

- `T1` - Use an SPDX license identifier.
- `T2` - Are events emitted for every storage mutating function?
- `T3` - Check for correct inheritance, keep it simple and linear. (SWC-125)
- `T4` - Use a `receive() external payable` function if the contract should accept transferred ETH.
- `T5` - Write down and test invariants about relationships between stored state.
- `T6` - Is the purpose of the contract and how it interacts with others documented using natspec?
- `T7` - The contract should be marked `abstract` if another contract must inherit it to unlock its full functionality.
- `T8` - Emit an appropriate event for any non-immutable variable set in the constructor that emits an event when mutated elsewhere.
- `T9` - Avoid over-inheritance as it masks complexity and encourages over-abstraction.
- `T10` - Always use the named import syntax to explicitly declare which contracts are being imported from another file.
- `T11` - Group imports by their folder/package. Separate groups with an empty line. Groups of external dependencies should come first, then mock/testing contracts (if relevant), and finally local imports.
- `T12` - Summarize the purpose and functionality of the contract with a `@notice` natspec comment. Document how the contract interacts with other contracts inside/outside the project in a `@dev` natspec comment.

## Project

- `P1` - Use the right license (you must use GPL if you depend on GPL code, etc).
- `P2` - Unit test everything.
- `P3` - Fuzz test as much as possible.
- `P4` - Use symbolic execution where possible.
- `P5` - Run Slither/Solhint and review all findings.