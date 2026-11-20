# Strategy submission

## เอกสารทั้งหมด

ทุกอย่างที่ต้องอ่านอยู่ใน repo นี้ — เริ่มจากไฟล์นี้ก่อน

| ไฟล์ | อ่านเมื่อ |
|---|---|
| `README.md` (ไฟล์นี้) | เริ่มต้น + กติกาการส่งงาน |
| [`docs/competitor_guide.md`](docs/competitor_guide.md) | ลำดับขั้นตอนตั้งแต่เขียนโค้ดจนดูผล · เกณฑ์ตัดสิน · อ่านผลใน `RESULTS.md` |
| [`docs/api_reference.md`](docs/api_reference.md) | รายละเอียดทุก method ของ `self.handler` พร้อมตัวอย่าง output · วิธีใช้ notebook (ภาษาอังกฤษ) |
| [`docs/WORKSHOP_CODE.md`](docs/WORKSHOP_CODE.md) | โค้ดทุกขั้นของ workshop — copy ไปวางเป็น `strategy/MyStrategy.py` ได้เลย |
| [`docs/get_historical_data.md`](docs/get_historical_data.md) | ดึงข้อมูลย้อนหลัง / ดัชนี SET50 · หุ้นที่มีข้อมูลย้อนหลังสั้น |
| [`docs/on_data_timeframe.md`](docs/on_data_timeframe.md) | `timeframe` กับแท่งราคาที่ `on_data` ได้รับ |
| [`docs/CHANGELOG.md`](docs/CHANGELOG.md) | มีอะไรเปลี่ยนในแต่ละเวอร์ชันของ `tradeSim` — อ่านทุกครั้งที่โหลด zip ตัวใหม่ |
| [`docs/examples/Test_TALib_strategy.py`](docs/examples/Test_TALib_strategy.py) | ตัวอย่างกลยุทธ์เต็มที่ใช้ TA-Lib (MACD + RSI, 8 หุ้น) |

> `docs/examples/` อยู่นอก `strategy/` ตั้งใจ — ถ้าจะลองรัน ให้ copy ไปไว้ใน `strategy/` ชั่วคราว
> แต่**อย่า commit** ไฟล์ที่สองใน `strategy/` เพราะ CI จะแดง (ดูกติกาข้อ 1 ด้านล่าง)

## เริ่มต้นใช้งาน (ทำครั้งเดียว)

ทำตามลำดับนี้ได้เลย **ยังไม่ต้องใช้ git** — git ใช้เฉพาะตอนส่งงานเข้าแข่ง
(ดูหัวข้อ [เข้าร่วมการแข่งขัน: ส่งงานผ่าน git](#เข้าร่วมการแข่งขัน-ส่งงานผ่าน-git) ด้านล่าง)

**0. เช็คว่าเครื่องมี Python 3.13**

เปิด Command Prompt (หรือ PowerShell) แล้วพิมพ์:
```bash
python --version
```
- ขึ้น `Python 3.13.x` = พร้อมแล้ว ไปข้อ 1 ได้เลย
- ขึ้น `'python' is not recognized` หรือเปิดหน้า Microsoft Store = ยังไม่มี Python ในเครื่อง →
  ติดตั้ง Python 3.13 จาก https://www.python.org/downloads/ (ไม่ใช่ Microsoft Store · ตอนติดตั้งติ๊ก
  **Add python.exe to PATH**) แล้วเปิดหน้าต่างใหม่มาเช็คอีกครั้ง — workshop ไม่ได้สอนลง Python
  ถ้าติดตั้งไม่ได้ให้ถามทีมงาน
- ขึ้นเวอร์ชันอื่น (เช่น `3.12` หรือ `3.14`) = ต้องใช้ **Python 3.13** เท่านั้น (library บางตัวยังไม่รองรับ 3.14) →
  ถ้าในเครื่องมี 3.13 อยู่ด้วย ใช้ `py -3.13` แทน `python` ในข้อ 3 · ถ้าไม่มี ติดตั้ง 3.13 ตามข้างบน
- macOS/Linux: `python3 --version`

**1. โหลด `tradeSim<version>.zip`** จาก**ลิงก์ Google Drive ที่ผู้จัดส่งให้**

**2. แตก zip ลงโฟลเดอร์ทำงาน** — ใช้โฟลเดอร์ที่ path สั้น ๆ เช่น `C:\TradeSim\workshop`
(ถ้าเข้าแข่งและ clone repo ทีมมาแล้ว ให้แตกลงในโฟลเดอร์ repo แทน — ดูหัวข้อเข้าร่วมการแข่งขัน)

> ⚠️ **ระวังแตกซ้อนโฟลเดอร์** — ใน zip ไฟล์อยู่ชั้นบนสุดเลย แต่ "Extract All" ของ Windows จะเสนอ path
> ที่มีโฟลเดอร์ `tradeSim<version>` ต่อท้ายให้ **ลบส่วนนั้นออกก่อนกด Extract** ให้ path เป็นโฟลเดอร์ทำงาน
> ตรง ๆ ไม่งั้น notebook จะไปอยู่ในโฟลเดอร์ย่อยและหาไฟล์กลยุทธ์ใน `strategy/` ไม่เจอ

หลังแตกเสร็จจะได้หน้าตาประมาณนี้:
```
<โฟลเดอร์ทำงาน>/
├── strategy/                  ← ไฟล์กลยุทธ์อยู่ตรงนี้ (มีตัวอย่างมาให้)
├── docs/                      ← เอกสาร
├── tradeSim/                  ← engine
├── marketInfo/                ← ข้อมูลตลาด
├── TradingSimulation.ipynb    ← notebook สำหรับรันกลยุทธ์
├── requirements.txt
└── requirements-optional.txt
```

**3. สร้าง virtual environment** (ครั้งเดียว) ในโฟลเดอร์ทำงาน — library ของ TradeSim จะไม่ไปปนกับ
Python ตัวหลักของเครื่อง และลบโฟลเดอร์ `.venv` ทิ้งแล้วสร้างใหม่ได้ถ้าพัง

เปิด Command Prompt **ในโฟลเดอร์ทำงาน** (หรือ `cd C:\TradeSim\workshop`) แล้วพิมพ์:
```bash
python -m venv .venv
.venv\Scripts\activate
```
- ข้อ 0 ขึ้นเวอร์ชันอื่นที่ไม่ใช่ 3.13: ใช้ `py -3.13 -m venv .venv` แทนบรรทัดแรก (เลือก 3.13 ให้แม้ในเครื่องจะมีหลายเวอร์ชัน)
- macOS/Linux: `python3 -m venv .venv` แล้ว `source .venv/bin/activate`
- activate แล้วหน้า prompt จะขึ้น `(.venv)` — **เปิด terminal ใหม่ทุกครั้งต้อง activate ใหม่**
- PowerShell ขึ้น `running scripts is disabled on this system` → รัน
  `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned` ครั้งเดียว หรือใช้ Command Prompt แทน
- **ต้องชื่อ `.venv`** — ถ้าภายหลังใช้ git `.gitignore` ของ repo ทีมกันชื่อนี้ไว้แล้ว ชื่ออื่นจะถูก
  `git add .` ขึ้นไปด้วย (~400MB)

**4. ติดตั้ง library** (ใน `(.venv)`)
```bash
python -m pip install -r requirements.txt
```
- เช็คว่ากำลังใช้ Python ใน `.venv` จริง: `python --version` ต้องขึ้น `Python 3.13.x`
  (ถ้าไม่ใช่ แปลว่ายังไม่ได้ activate — กลับไปข้อ 3)
- ได้ engine + `numpy` `pandas` + technical analysis + Jupyter (โหลด ~80MB) พอสำหรับเริ่ม
- library ML/สถิติ/optimization อยู่ใน `requirements-optional.txt` (~240MB) ลงเมื่อกลยุทธ์จะ import
  จริง: `python -m pip install -r requirements-optional.txt` (ดู [Library ที่มีให้ใช้แล้ว](#library-ที่มีให้ใช้แล้ว))
- เช็คหลังติดตั้ง: `python -c "import pandas, talib; print('ok')"` — ถ้าขึ้น
  `DLL load failed ... Application Control policy` แปลว่า Smart App Control ของ Windows 11 บล็อกอยู่
  ต้องปิดใน Windows Security (ปิดแล้วเปิดกลับไม่ได้) หรือใช้ WSL แทน
- ติดตั้งไม่ผ่านด้วย `OSError ... Long Path` = path โฟลเดอร์ลึกเกิน ย้ายไปที่สั้น ๆ เช่น
  `C:\TradeSim\workshop` แล้วสร้าง `.venv` ใหม่

**5. เปิด `TradingSimulation.ipynb`** แล้วรันกลยุทธ์ตัวอย่างดูก่อน
- VS Code → เปิด `TradingSimulation.ipynb` → **Select Kernel → Python Environments → `.venv`**
  (ไม่เลือก = ใช้ Python ตัวหลัก แล้ว `import talib` ไม่เจอ ทั้งที่เพิ่งติดตั้ง)
- หรือพิมพ์ `jupyter notebook` ใน terminal ที่ activate แล้ว

> **อัปเดต engine รุ่นใหม่** — แตก zip ตัวใหม่ทับลงไปซ้ำได้เลย ไฟล์กลยุทธ์ของตัวเองใน `strategy/`
> ไม่โดนทับ (คนละชื่อไฟล์กัน)

> **อยากได้ข้อมูลย้อนหลังล่าสุด** (เอาไปวิเคราะห์/เทรนโมเดล) — ในโฟลเดอร์ Google Drive เดียวกันมี
> `tradeSim_history.zip` ที่ผู้จัดอัปเดตให้ทุกเช้าวันทำการ แตกทับลงในโฟลเดอร์ทำงานแบบเดียวกัน ไม่ต้อง commit
> → รายละเอียดและข้อควรรู้: [`docs/get_historical_data.md`](docs/get_historical_data.md#อัปเดตข้อมูลย้อนหลังในเครื่องตัวเอง--tradesim_historyzip)

## เข้าร่วมการแข่งขัน: ส่งงานผ่าน git

ทำส่วนนี้**เฉพาะทีมที่ลงทะเบียนแข่ง** — ระบบดึงกลยุทธ์จาก repo ของทีมบน GitHub ไปรันแข่ง
(เข้าแค่ workshop ไม่ต้องทำ)

**ก. เตรียม git** (ครั้งเดียวต่อเครื่อง)

- ติดตั้ง git จาก https://git-scm.com/download/win แล้วเช็คด้วย `git --version`
- ตั้งชื่อที่จะติดไปกับ commit — ไม่ตั้ง `git commit` ครั้งแรกจะล้มด้วย `Please tell me who you are`
  ```bash
  git config --global user.name "ชื่อของคุณ"
  git config --global user.email "อีเมลที่ใช้กับ GitHub"
  ```
- **กดรับคำเชิญเข้า repo ทีม** ในอีเมล หรือที่ https://github.com/notifications — ยังไม่รับ
  `git clone` จะขึ้น `Repository not found` ทั้งที่ URL ถูก
- ⚠️ **ตอน git ขอ login ให้ใช้บัญชี GitHub เดียวกับที่ส่งมาใน sheet ลงทะเบียน** — สิทธิ์เข้า repo ทีม
  ผูกกับบัญชีนั้นบัญชีเดียว login บัญชีอื่นจะเจอ `Repository not found` ตอน clone หรือ `Permission denied`
  ตอน push
  - เครื่องที่เคย login GitHub บัญชีอื่นไว้ git จะใช้บัญชีเดิมเองโดยไม่ถาม → เปิด **Credential Manager →
    Windows Credentials** ลบรายการ `git:https://github.com` แล้วลองใหม่ จะมีหน้าต่างให้ login ใหม่

**ข. Clone repo ของทีม แล้วใช้เป็นโฟลเดอร์ทำงาน**
```bash
git clone https://github.com/<org>/<UNI>-<TEAM>.git
cd <UNI>-<TEAM>
```
แล้วทำข้อ 1–5 ของ [เริ่มต้นใช้งาน](#เริ่มต้นใช้งาน-ทำครั้งเดียว) **ในโฟลเดอร์ repo นี้** (แตก zip ทับลงไป ·
สร้าง `.venv` ใหม่ในนี้) — ถ้าเคยทำใน `C:\TradeSim\workshop` มาแล้ว ให้ copy ไฟล์กลยุทธ์ของตัวเองจาก
`strategy/` ของโฟลเดอร์เดิมมาไว้ใน `strategy/` ของ repo

หลังแตก zip ในโฟลเดอร์ repo จะได้แบบนี้:
```
<UNI>-<TEAM>/
├── strategy/                  ← ไฟล์กลยุทธ์ของทีมอยู่ตรงนี้ (อันนี้ถูก commit)
├── docs/                      ← เอกสาร (มากับ repo)
├── README.md                  ← ไฟล์นี้
├── tradeSim/                  ┐
├── marketInfo/                │ มากับ zip — .gitignore กันไว้แล้ว
├── TradingSimulation.ipynb    │ ไม่ถูก commit ขึ้น repo
├── requirements.txt           │
└── requirements-optional.txt  ┘
```

`.gitignore` ที่มากับ repo กันไฟล์ฝั่ง engine ไว้ให้แล้ว **สั่ง `git add .` ได้ตามปกติ
โดยไม่ต้องกลัวว่าจะเผลอ push engine หรือข้อมูลตลาดขึ้นมา** · ส่งงานตาม [กติกาการส่งงาน](#กติกาการส่งงาน) ด้านล่าง

> **ทำแบบนี้แล้วได้อะไร** — ไฟล์ที่ทดสอบในเครื่องกับไฟล์ที่ push ขึ้นไปคือไฟล์เดียวกันเสมอ
> ไม่ต้องก๊อปไปมาระหว่างโฟลเดอร์ ซึ่งเป็นต้นเหตุคลาสสิกของ "ที่เครื่องรันได้ แต่ CI ตก"

## กติกาการส่งงาน

1. เขียนไฟล์กลยุทธ์ของทีม **1 ไฟล์** ไว้ที่ `strategy/<ชื่อไฟล์ของทีม>.py`
   (ตั้งชื่อไฟล์อะไรก็ได้ แต่ต้องมีแค่ไฟล์เดียว)

   > ⚠️ **แค่ไฟล์เดียวจริง ๆ** — วาง `.py` ตัวที่สองไว้ใน `strategy/` เมื่อไหร่ CI แดงทันที
   > ด้วยข้อความ `Expected exactly one strategy file ... found 2` ไม่ว่าจะเป็นโมดูลย่อยที่แยก
   > โค้ดออกมา หรือไฟล์สำรองอย่าง `MyTeam_backup.py` ที่ไม่ได้ใช้แล้วก็ตาม — `git add .`
   > ไม่เตือนอะไรเลย จะรู้ตัวอีกทีตอน Checks ขึ้นแดง
   > (ไฟล์ที่ระบบยกเว้นให้มีแค่ 4 ตัวที่มากับ template: `Strategies_template.py`,
   > `Example_strategy.py`, `Example_historical_strategy.py`, `__init__.py`)
   >
   > - อยากแนบข้อมูล/พารามิเตอร์/โมเดล → ใช้ `.json` หรือ `.skops` แล้วโหลดผ่าน `handler`
   >   (ดูหัวข้อ "แนบไฟล์ config/โมเดลมากับกลยุทธ์" ด้านล่าง) — `.csv`/`.md` ที่วางไว้ข้าง ๆ
   >   **ไม่ถูกส่งไปรันด้วย** และจะเปิดอ่านเองก็ไม่ได้ เพราะ `open()` อยู่ในรายการห้ามเรียก
   > - **ทดลองอะไรนอก `strategy/` ได้ตามสบาย** — สคริปต์ train โมเดล, โน้ตบุ๊ควิเคราะห์,
   >   กราฟ, ข้อมูลดิบ จะ commit ขึ้น repo ไปด้วยก็ไม่มีปัญหา CI ดูเฉพาะใน `strategy/` เท่านั้น
   > - **แต่ไฟล์กลยุทธ์ต้องรันได้ด้วยตัวเองไฟล์เดียว ห้าม import ไฟล์อื่นของทีม** ไม่ว่าไฟล์นั้น
   >   จะอยู่ใน `strategy/`, ในโฟลเดอร์ย่อย หรือที่อื่นใน repo — ระบบคัดลอกไปรันแค่ไฟล์กลยุทธ์
   >   กับไฟล์แนบที่อนุญาต โค้ดที่พึ่งไฟล์อื่นจะพังบนเครื่องผู้จัด ทั้งที่เครื่องตัวเองรันผ่าน
   > - **ถ้าจำเป็นต้องมี helper จริง ๆ ให้ติดต่อผู้จัดก่อน** ส่งเป็น request หรือ message มา
   >   ให้ทีมงานตรวจ อย่าเพิ่ง push ไฟล์ที่สองขึ้นไปเอง
2. Class ในไฟล์นั้น **ต้องชื่อตรงกับชื่อไฟล์** เช่นไฟล์ `MyTeam.py` ต้องมี `class MyTeam(...)`
3. ต้อง inherit จาก `Strategy_template`, เรียก `super().__init__(owner, strategy_name, handler, timeframe)`
   โดยระบุ `timeframe` เอง (เช่น `"1min"`, `"5min"`, `"1D"`) และ implement `on_data(self, bar)` —
   `bar` คือแท่งราคา OHLCV ของ timeframe ที่เลือก **ไม่ใช่ tick ดิบ** ดูรายละเอียด/ตัวอย่างที่
   [`docs/on_data_timeframe.md`](docs/on_data_timeframe.md)
4. **ห้ามแก้ไข** `strategy/Strategies_template.py`
5. **ห้าม import** โมดูลที่แตะ filesystem/network/process (`os`, `sys`, `subprocess`,
   `socket`, `requests`, `urllib`, `pickle`, ฯลฯ) และห้ามเรียก `eval`, `exec`, `open`, `getattr`
   - **ห้ามอ่าน/เขียนไฟล์ผ่าน library ด้วย** เช่น `pd.read_csv()`, `np.load()`, `df.to_csv()` —
     ข้อมูลตลาดให้ดึงผ่าน `self.handler.get_historical_data()` ไฟล์ของทีมเองโหลดผ่าน `handler`
     (ดูหัวข้อ "แนบไฟล์ config/โมเดลมากับกลยุทธ์")
   - ห้าม relative import (`from . import helper`) — ไฟล์กลยุทธ์ต้องอยู่ได้ด้วยตัวเองไฟล์เดียว
6. push เข้า branch **`main`** ของทีม แล้วดูผลตรวจที่แท็บ **Checks** บนหน้า commit ของตัวเอง
   - **เขียว** = ผ่าน ระบบจะเอาโค้ดนี้ไปใช้แข่ง
   - ⏰ **เส้นตาย 09:30 น. ของวันเทรด** — push ก่อน 09:30 ใช้กับวันนั้น · push ตั้งแต่ 09:30 ใช้กับวันเทรดถัดไป
     (นับเวลาที่ GitHub ได้รับ push ไม่ใช่เวลาใน commit) รายละเอียดที่ [`docs/competitor_guide.md`](docs/competitor_guide.md)
   - **แดง** = ระบบ**ไม่เอาไปใช้** แข่งต่อด้วยเวอร์ชันล่าสุดที่เคยผ่าน — เปิด log ใน Checks ดูสาเหตุ แก้แล้ว push ใหม่
   - ใช้แข่งเฉพาะโค้ดบน `main` — branch อื่นมี CI ตรวจให้ แต่ไม่ถูกเอาไปแข่ง

## Library ที่มีให้ใช้แล้ว

ติดตั้งไว้ล่วงหน้าบน runner ให้หมดแล้ว **import ใช้ได้เลยไม่ต้องขอ ไม่ต้องมี `requirements.txt`
ของทีมเอง** (ทีมสั่ง `pip install` เองไม่ได้อีกต่อไป — ตัดออกเพื่อความปลอดภัย เพราะการติดตั้ง
package รันโค้ดได้ตอนติดตั้ง ควบคุมยากกว่าตรวจไฟล์ `.py` ธรรมดา)

ที่เครื่องตัวเอง `requirements.txt` ลงให้แค่ `numpy` `pandas` `talib` `ta` `finta` —
**ตัวที่มี ★ ต้องลง `requirements-optional.txt` เพิ่ม** ก่อนถึงจะ import ได้ในเครื่องตัวเอง:

- **ตัวเลข/สถิติ**: `numpy`, `pandas`, `scipy` ★, `statsmodels` ★
- **Machine learning** ★: `scikit-learn`, `xgboost`, `lightgbm`, `catboost`
- **Technical analysis**: `TA-Lib` (import ว่า `talib`), `ta`, `finta`
- **Time series** ★: `pmdarima`, `arch`
- **Portfolio optimization** ★: `cvxpy`, `PuLP`, `pyportfolioopt` (import ว่า `pypfopt`)
- **Plotting** (ใช้ตอนพัฒนา/ดู debug ที่เครื่องตัวเองเท่านั้น รันจริงบน CI ไม่มีจอให้แสดงผล): `matplotlib` ★
- **Symbolic math** ★: `sympy`
- **Safe model serialization** (ดูหัวข้อ "แนบไฟล์ config/โมเดลมากับกลยุทธ์" ด้านล่าง): `skops` ★

**ต้องการ library อื่นนอกเหนือจากนี้** ติดต่อผู้จัดการแข่งขันโดยตรง (ไม่ใช่ automated flow —
ไม่มีกลไกให้ทีมขอ/ติดตั้งเองผ่าน push อีกแล้ว)

## แนบไฟล์ config/โมเดลมากับกลยุทธ์

วางไฟล์เพิ่มไว้ในโฟลเดอร์ `strategy/` เดียวกับไฟล์ `.py` ของทีมได้เลย (คนละไฟล์กัน ไม่นับเป็น
ไฟล์กลยุทธ์ที่สอง) แล้วโหลดผ่าน `self.handler` — ไฟล์กลยุทธ์เรียก `open()` เองไม่ได้ (ข้อ 5 ด้านบน)
เลยต้องผ่าน method พวกนี้แทน:

| Method | ใช้กับ | ตัวอย่าง |
|---|---|---|
| `handler.load_json(filename)` | ไฟล์ `.json` (config/พารามิเตอร์) | `self.threshold = handler.load_json("config.json")["threshold"]` |
| `handler.load_model(filename)` | ไฟล์ `.skops` (โมเดล scikit-learn ที่ train แล้ว) | `self.model = handler.load_model("model.skops")` แล้วเรียก `.predict()` ได้เลย |
| `handler.get_companion_path(filename)` | ไฟล์ `.json`/`.txt`/`.ubj`/`.cbm` ที่จะยื่นให้ library ของตัวเอง | `xgb.Booster().load_model(handler.get_companion_path("model.json"))` |

**ห้ามแนบ `.pkl`/`.pickle`/`.joblib`/`.h5`/`.pt`/`.pth`** — ไฟล์รูปแบบนี้รันโค้ดได้ตอนโหลด (ไม่ใช่
แค่เก็บข้อมูล) ระบบจะตรวจไม่ผ่านทันทีถ้าเจอ ให้ใช้ทางเลือกที่ปลอดภัยแทน:
- โมเดล **scikit-learn**: เซฟด้วย `skops` (ไม่ใช่ `pickle`/`joblib`) ที่เครื่องตัวเอง แล้ว commit
  ไฟล์ `.skops` ที่ได้:
  ```python
  import skops.io as sio
  sio.dump(model, "strategy/model.skops")
  ```
- โมเดล **xgboost/lightgbm/catboost**: ใช้ native format ของตัวเองอยู่แล้ว (`Booster.save_model("model.json")`
  สำหรับ xgboost, `Booster.save_model("model.txt")` สำหรับ lightgbm, `model.save_model("model.cbm")`
  สำหรับ catboost) ไม่ต้องพึ่ง skops

ไฟล์แต่ละไฟล์ห้ามเกิน 20MB

## พัฒนา/ทดสอบกลยุทธ์ที่เครื่องตัวเอง

ใช้ `TradingSimulation.ipynb` ที่มากับ zip (ข้อ 1–5 ของเริ่มต้นใช้งาน) — ผลจากการรันที่เครื่องตัวเอง
ใช้สำหรับซ้อม/debug เท่านั้น **ผลตัดสินจริงมาจากการรันของผู้จัดหลัง push เท่านั้น**

- รันเซลล์ **2 → 8 เรียงลงมาทุกครั้ง** (เซลล์ 1 ครั้งแรกครั้งเดียว) — แก้วันที่ซ้อมในเซลล์ 2
  ชื่อไฟล์กลยุทธ์ในเซลล์ 3
- **`fresh_start` ในเซลล์ 3**: `True` (ค่าเริ่มต้น) เริ่มพอร์ต 10 ล้านใหม่ทุกครั้ง · `False` รันต่อจาก
  พอร์ตที่บันทึกไว้ใน `result/` จากรอบก่อน — ใช้ตอนซ้อมทีละวัน
- **เซลล์ท้าย "Run Several Days in a Row"** รันหลายวันต่อกันในครั้งเดียว พอร์ตต่อเนื่องข้ามวัน
  เหมือนการแข่งจริง
- ⚠️ **CI เขียวไม่ได้แปลว่ากลยุทธ์ทำงานถูก** — ตอน CI ไม่มีข้อมูลย้อนหลัง `get_historical_data()`
  คืน `[]` เสมอ โค้ดที่คำนวณ indicator จึงไม่เคยถูกรันใน CI ต้องรันใน notebook ให้เห็นผลเองก่อน push

## Method ที่ใช้ได้ผ่าน `self.handler`

| Method | Parameter |
|---|---|
| `create_order_to_limit` | `(volume, price, side, symbol)` |
| `create_order_at_market` | `(volume, side, symbol)` |
| `get_open_orders` | `(symbol=None)` — คำสั่ง limit ที่ยังค้างใน order book |
| `cancel_order` | `(order_number)` |
| `cancel_all_orders` | `(symbol=None)` |
| `check_port_has_stock` | `(symbol, volume)` |
| `get_stock_by_symbol` | `(symbol)` |
| `get_all_stocks_info` | `()` |
| `get_portfolio_info` | `()` |
| `get_cash_balance` | `()` |
| `get_historical_data` | `(symbol, n, timeframe="1D")` — ดูรายละเอียด/ตัวอย่างที่ [`docs/get_historical_data.md`](docs/get_historical_data.md) |

ดู `strategy/Example_strategy.py` ประกอบ — ถ้าอยากดูตัวอย่างที่เรียก `get_historical_data` จริง ดูที่
`strategy/Example_historical_strategy.py` (SMA 5 วันแบบง่าย ๆ)
