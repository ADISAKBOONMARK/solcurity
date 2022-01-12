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
- `F5` - ตรวจสอบความถูกต้องของการ Validate all parameters ว่าทำได้ถูกต้องหรือไม่ แม้ว่า Function จะสามารถเรียกได้เฉพาะบุคคลที่เชื่อถึอได้ เช่น Owner, Admin, Operator เป็นต้น
- `F6` - ตรวจสอบผลกระทบว่าเป็นไปตามเงื่อนไขหรือไม่ ? ([SWC-107](https://swcregistry.io/docs/SWC-107))
- `F7` - ตรวจสอบการทำงานของ Function ที่มีผลการทำงานต่อเนื่องกัน ว่ามีความเสี่ยงที่จะถูกโจมตีหรือไม่ ([SWC-114](https://swcregistry.io/docs/SWC-107))
- `F8` - ปัญหา Gas ไม่เพียงพอเมือต้องทำการเรียกใช้งาน Smartcontracts มากกว่า 1 ใบ ([SWC-126](https://swcregistry.io/docs/SWC-126))
- `F9` - Modifiers ทำงานถูกต้องหรือไม่ เช่น `onlyOwner`, `requiresAuth` เป็นต้น
- `F10` - การ Return ค่า ถูกต้องตามที่กำหนดหรือไม่ ?
- `F11` - เขียน Test สำหรับตรวจสอบค่าตัวแปรก่อนที่จะเรียก Function ในการเปลี่ยนแปลงค่า
- `F12` - เขียน Test สำหรับตรวจสอบค่าตัวแปรหลังจากเรียก Function ในการเปลี่ยนแปลงค่า
- `F13` - ใส่ใจในการตั้งชื่อ Function เพราะจะทำให้คนทั่วไปเข้าใจการทำงานของ Function ที่ควรจะเป็น
- `F14` - วัตถุประสงค์ของ Arguments, Return values, Side effects และอื่น ๆ ถูกอธิบายไว้โดยใช้ [Natspec](https://docs.soliditylang.org/en/v0.8.10/natspec-format.html) หรือไม่ ?
- `F15` - ถ้า Function อนุญาต User ดำเนินการ อย่าเข้าใจว่า `msg.sender` คือ User จริง เพราะอาจเป็น Smartcontracts ได้
- `F16` - ถ้า Smartcontracts ไม่ได้ Requires Arguments ให้ตรวจสอบ Arguments ของ `initialized` เช่น ไม่ควรใช้ `owner == address(0)` หรือเหตุการณ์อื่น ๆ ที่คล้ายกัน
- `F17` - ใช้ `private` ในกรณีที่ต้องการป้องกันการเข้าถึงจาก Child contracts และใช้ `internal` ในกรณีที่ต้องการป้องกันการเข้าถึงจาก Contracts อื่น ยกเว้น Child contracts
- `F18` - ใช้ `virtual` ถ้าปลอดภัย เพราะบางที Child contracts อาจทำการ Override การทำงานของ Function

## Modifiers

- `M1` - ไม่ทำการแก้ไข Storage (ยกเว้น Reentrancy lock)
- `M2` - มีการเรียกไปยังภายนอกหรือไม่ ? 
- `M3` - วัตถุประสงค์ของ Modifiers และอื่น ๆ ถูกอธิบายไว้โดยใช้ [Natspec](https://docs.soliditylang.org/en/v0.8.10/natspec-format.html)?

## Code

- `C1` - ใช้ SafeMath หรือ Solidity 0.8.x สำหรับตรวจสอบ Math ? ([SWC-101](https://swcregistry.io/docs/SWC-101))
- `C2` - Storage ถูกเรียกหลายครั้งหรือไม่ ?
- `C3` - มีการใช้งาน Loops/Arrays มาก ๆ หรือไม่ ? อาจทำให้เกิด DoS ([SWC-128](https://swcregistry.io/docs/SWC-128))
- `C4` - ใช้ `block.timestamp` เฉพาะช้วงเวลาเท่านั้น ([SWC-116](https://swcregistry.io/docs/SWC-116))
- `C5` - ไม่ใช้ block.number สำหรับเวลาที่ผ่านไป ([SWC-116](https://swcregistry.io/docs/SWC-116))
- `C7` - หลีกเลี่ยงการ delegatecall ในทุกที่ที่ทำได้ โดยเฉพาะกับ Contracts ภายนอก (แม้ว่าจะเชื่อถือได้ก็ตาม) ([SWC-112](https://swcregistry.io/docs/SWC-112))
- `C8` - อย่าอัปเดต Length ของ Arrays ขณะวนซ้ำ
- `C9` - ไม่ใช้ `blockhash()` สำหรับ Randomness ([SWC-120](https://swcregistry.io/docs/SWC-120))
- `C10` - Signatures ได้รับการป้องกันจากการทำซ้ำด้วย nonce และ `block.chainid` ([SWC-121](https://swcregistry.io/docs/SWC-121))
- `C11` - ตรวจสอบให้แน่ใจว่า Signatures ทั้งหมดใช้ EIP-712. ([SWC-117](https://swcregistry.io/docs/SWC-117) [SWC-122](https://swcregistry.io/docs/SWC-122))
- `C12` - ผลลัพธ์ของ `abi.encodePacked()` ไม่ควร hashed ถ้าใช้มากกว่า 2 dynamic ให้ใช้ `abi.encode()` แทน ([SWC-133](https://swcregistry.io/docs/SWC-133))
- `C13` - ระวังกับ Assembly ไม่ใช้ข้อมูลใด ๆ โดยพลการ ([SWC-127](https://swcregistry.io/docs/SWC-127))
- `C14` - อย่าถือว่ายอดคงเหลือ ETH เป็นแบบเฉพาะเจาะจง ([SWC-132](https://swcregistry.io/docs/SWC-132))
- `C15` - หลีกเลี่ยงการ Function ที่ใช้ Gas มากจนเกินไป ([SWC-126](https://swcregistry.io/docs/SWC-126))
- `C16` - Private data ไม่เป็น Private. ([SWC-136](https://swcregistry.io/docs/SWC-136))
- `C17` - การอัปเดต struct/array ใน memory จะไม่ถูกแทนที่ใน storage
- `C18` - ไม่ประกาศตัวแปรถ้าไม่ได้ใช้งาน ([SWC-119](https://swcregistry.io/docs/SWC-119))
- `C19` - ไม่เปลี่ยนแปลงตัวแปร Function
- `C20` - การคำนวณมูลค่าทันทีถูกกว่าการจัดเก็บหรือไม่ ?
- `C21` - ตัวแปรถูกอ่านจาก Contract ปัจจุบันหรือไม่ ? (master vs. clone)
- `C22` - ใช้ตัวดำเนินการเปรียบเทียบอย่างถูกต้อง (`>`, `<`, `>=`, `<=`) โดยเฉพาะอย่างยิ่งกับการป้องกันข้อผิดพลาด
- `C23` - ใช้ตัวดำเนินการเชิงตรรกะอย่างถูกต้อง (`==`, `!=`, `&&`, `||`, `!`) โดยเฉพาะอย่างยิ่งกับการป้องกันข้อผิดพลาด
- `C24` - คูณก่อนหารเสมอ เว้นแต่การคูณมีโอกาส overflow
- `C25` - Magic numbers ถูกแทนที่ด้วยค่าคงที่ด้วยชื่อที่เข้าใจได้ง่ายหรือไม่ ?
- `C26` - หากผู้รับ ETH มีฟังก์ชัน fallback เพื่อตอบกลับ จะทำให้เกิด DoS ได้หรือไม่ ? ([SWC-113](https://swcregistry.io/docs/SWC-113))
- `C27` - ใข้ SafeERC20 หรือตรวจสอบค่าที่ส่งคืนได้อย่างปลอดภัย
- `C28` - ไม่ใช้ `msg.value` ใน Loop
- `C29` - ไม่ใช้ `msg.value` หาก Recursive delegatecalls ถ้าเป็นไปได้ (เช่น ถ้า Contract inherits `Multicall`/`Batchable`).
- `C30` - อย่าเข้าใจว่า `msg.sender` คือ User จริง เพราะอาจเป็น Smartcontracts ได้
- `C31` - ไม่ใช้ `assert()` เว้นแต่เพื่อการตรวจสอบที่คลุมเครือหรือเป็นทางการ ([SWC-110](https://swcregistry.io/docs/SWC-110))
- `C32` - ไม่ใช้ `tx.origin` ในการ Authorization ([SWC-115](https://swcregistry.io/docs/SWC-115))
- `C33` - ไม่ใช้ `address.transfer()` หรือ `address.send()` ใช้ `.call.value(...)("")` แทน ([SWC-134](https://swcregistry.io/docs/SWC-134))
- `C34` - เมื่อใช้ Low-level calls ต้องแน่ใจว่า Contract มีอยู่ก่อนการโทร
- `C35` - เมื่อเรียก Function ด้วย Parameters หลาย ๆ ตัว ใช้ชื่อที่สื่อความหมายชัดเจน
- `C36` - ไม่ใช้ Assembly สำหรับ create2 เสนอให้ใช้ Syntax ที่ทันสมัยกว่า
- `C37` - ไม่ใช้ Assembly เพื่อเข้าถึง chainid หรือ Contract code/size/hash เสนอให้ใช้ Syntax ที่ทันสมัยกว่า
- `C38` - ใช้ `delete` คีย์เวิร์ดเมื่อตั้งค่าตัวแปรให้เป็นค่าศูนย์ (`0`, `false`, `""`, อื่น ๆ)
- `C39` - Comment ว่า "why" ให้มากที่สุด
- `C40` - Comment ว่า "what" หากใช้ Syntax ที่คลุมเครือ หรือเขียน Code ที่แปลกไปจาก Case อื่น ๆ
- `C41` - Comment คำอธิบาย + ตัวอย่าง Inputs/Outputs ต่อจากสมการเชิงคณิตศาสตร์ที่ซับซ้อน
- `C42` - Comment คำอธิบาย ทุกที่ที่สามารถ Optimizations ได้ พร้อมกับการประมาณการของ Gas ที่ประหยัดได้
- `C43` - Comment คำอธิบาย ทุกที่ที่จงใจหลีกเลี่ยงการ Optimizations พร้อมกับการประมาณการของ Gas ที่จะไม่ประหยัด
- `C44` - ใช้ `unchecked` ที่ไหนที่ Overflow/Underflow เป็นไปไม่ได้ หรือที่ไหนที่ Overflow/Underflow ไม่สมเหตุสมผลต่อการใช้งานของผู้ใช้ (counters, อื่น ๆ). Comment คำอธิบาย ทุกที่ที่สามารถ `unchecked` พร้อมกับการประมาณการของ Gas ที่ประหยัดได้ (หากเกี่ยวข้อง).
- `C45` - อย่าพึ่งพากฎการมาก่อนตัวดำเนินการเลขคณิตของ Solidity นอกจากการใช้วงเล็บเพื่อลบล้างลำดับความสำคัญของตัวดำเนินการเริ่มต้นแล้ว ควรใช้วงเล็บเพื่อเน้นย้ำด้วย
- `C46` - Logical/Comparison operators (`&&`/`||`/`>=`/`==`/ อื่น ๆ) ไม่ควรมี Side-effects.
- `C47` - ทุกที่ที่มีการดำเนินการทางคณิตศาสตร์ที่อาจส่งผลให้เกิดการสูญเสีย ตรวจสอบให้แน่ใจว่าเป็นประโยชน์ต่อ Actors ในระบบ และจัดทำเป็นเอกสารพร้อมข้อคิดเห็น
- `C48` - Comment เหตุผลว่าทำไมต้องใช้ Reentrancy lock ในที่ที่จำเป็นต้องใช้ ใน natspec `@dev`
- `C49` - เมื่อใช้ fuzzing functions เพื่อดำเนินการเฉพาะ เช่น Numerical ranges ใช้ Modulo สำหรับ fuzzer's inputs (เช่น `x = x % 10000 + 1` จำกัด  1 ถึง 10,000)
- `C50` - ใช้ Ternary แทนในการอธิบาย logic ที่เข้าใจง่าย 
- `C51` - เมื่อ Operating address มากกว่าหนึ่ง ให้ตั้งคำถามเสมอว่าถ้าหากพวกเข้าเป็นคนเดียวกันหรือ Address เดียวกัน

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