
# Hive-APP 
> phase 1 @develop เริ่มโปรเจค @12 DEC 2017 

## <a name='TOC'>Hive React/Redux/JSX Style Guide</a>

  1. [Class vs Stateless](#class-vs-stateless)
  1. [Redux](#redux)
  1. [Naming](#naming)
  1. [Function](#function)
  1. [Workspace](#workspace)
  1. [Written Style](#written-style)
  1. [Dependency](#dependency)

## Class VS Stateless
  - หนึ่งคอมโพเน้นท์ ต่อหนึ่งไฟลล์เท่านั้น
  - แยก Container Component สำหรับ Class และ Connect Redux ให้เลือกใช้ `class extends React.Component` *
  - แยก Presentation Component สำหรับแต่ละ View และ Style ให้เลือกใช้ `export default const = () =>` *
  - ลำดับโดย `initialState, Lifecycle, JSFunctions, JSX, mapDispatchToProps, mapStateToProps และ export`
  - ควรใส่วงเล็บครอบ JSX และ Condition(?!&) ไว้ ในกรณีที่โค้ดมีมากกว่าหนึ่งบรรทัด
  - *การใช้ Container Component ให้พึงคิดเสมอว่า จะทำให้ แอพฯเรนเดอร์ หนึ่งครั้ง เพราะฉะนั้นถ้า ไม่ควรใช้ Container Component ใน Routing ที่ซ้ำกัน จะทำให้เกิดการ เรนเดอร์ที่ซ้ำซ้อน หรือเกิดการกระพริบได้*

```javascript
    // Container Component
    class MyComponent extends React.Component {
    
      state = {
	      //..
      }
      
      componentDidMount() {
	      //..
      }
      
      _testFunction = () => {
	      //..
      }
      
      render() {
        return (
               <View>
                <MySubComponent  
                    a={...props} //จาก mapStateToProps ไปใช้ ควรเลือกเฉพาะ props ที่คอมโพเน้นท์ลูกต้องการเท่านั้น
                    b={...state} //จาก initial State ไปใช้ ควรเลือกเฉพาะ props ที่คอมโพเน้นท์ลูกต้องการเท่านั้น
                    c={this._testFunction} //props ฟังก์ชั่นไปใช้
                />
               </View>
               )
      }
      
    }
   
     //react-redux
     const mapDispatchToProps = dispatch => {
        return {
           //..
        }
      }
      
     const mapStateToProps = state => {
        return {
           //..
        }
      }
	
	export default connect(mapStateToProps, mapDispatchToProps)(MyComponent)
```

 - การใช้ `ฟังก์ชั่น, Props หรือ State` ใน Presentation Component ให้ส่ง props มาจาก Container Component

```javascript
    // Stateless Component
    
    export default const MySubComponent = ({a,b,c}) => (
        <View>
            <Text>{a.yourProps}</Text>
            <Text>{b.yourState}</Text>
            <Button onPress={c._testFunction} /> //อย่า void เพราะเมื่อ onPress ปุ่มจะ void ให้
        <View>
    )
	
    const styles = StyleSheet.create({
      //..
    )}
```

## Redux
  - แยก Reducer ตาม Routing ของ Navigation
  - ActionType ควรตั้งชื่อในรูปแบบ UPPERCASE (ตัวอักษรพิมพ์ใหญ่ทั้งหมด) และตั้งชื่อให้สัมพันธ์กับ Reducer
  - ถ้าจะส่งค่าเข้า reducer ให้ dispash ค่ามากับ payload ไม่ควรเก็บค่าไว้ใน Reducer
    
```javascript
    // รูปแบบการเขียน Reducer
    const setHeader = function(state = '', action) {
      let { title, desc, type } = action //ย่อรูปออปเจ็ค
        switch(type) {
        case 'HEADER_TITLE':
          return title
        case 'HEADER_DESC':
          return desc
        defult : 
          return state
     }
     
     //รูปแบบการเรียกใช้ reducer
     const mapDispatchToProps = dispatch => {
        return {
          setHeaderMenubar: () => dispatch({ type: 'HEADER_TITLE', title: "หน้าหลัก" }),
          setHeaderMenubarDesc: () => dispatch({ type: 'HEADER_DESC', desc: "สาขา สยามสแคร์วัน" }),
        }
      }
     
     //รูปแบบการเรียกใช้ State จาก Redux
     const mapStateToProps = state => {
          return {
            headerTitle : state.title,
            headerDesc  : state.desc
          }
      }
    
```

## Naming
  - **ชื่อไฟล์** 
    - ควรตั้งชื่อในรูปแบบของ PascalCase (ขึ้นต้นทุกคำด้วยตัวใหญ่) ทั้งหมด ตัวอย่างเช่น `OrderListTable.js`
    - ชื่อคอมโพเน้นท์**: ควรตั้งให้เหมือนชื่อไฟล์ ตัวอย่างเช่น ไฟล์ชื่อ `ReservationCard.jsx` ควรตั้งชื่อคอมโพเน้นท์เป็น `ReservationCard`
  - **ชื่อตัวแปร** 
    - ควรตั้งชื่อในรูปแบบของ PascalCase (ขึ้นต้นทุกคำด้วยตัวใหญ่) สำหรับชื่อของคอมโพเน้นท์ต่างๆของ React
    - ควรตั้งชื่อในรูปแบบของ camelCase (ขึ้นต้นด้วยตัวเล็กและคำต่อไปขึ้นต้นด้วยตัวใหญ่) สำหรับชื่อของ `ตัวแปร, ฟังก์ชั่น, Props, State และ Reducer`
  - การตั้งชื่อควรตั้งให้สื่อความหมาย ถึงการกระทำหรือหน้าที่ให้ชัดเจนทุกตัวแปล เป็นฟังก์ชั่นกรุ๊ปไหน ทำหน้าที่อะไร
  
```javascript
    // การตั้งชื่อไฟลล์ และ การตั้งชื่อ Component
    import OrderGetListTable from './OrderGetListTable' // ควรใช้ PascalCase
    
    // การตั้งชื่อ Component
    class MyComponent extends React.Component // ควรใช้ PascalCase
    export default const MySubComponent = () => // ควรใช้ PascalCase

    // การตั้งชื่อฟังก์ชั่น
    orderGetListTable = () => // ควรใช้ camelCase
    
    // ไม่เหมาะสม
    fucking = () => //ความหมายผิดเพี้ยน
    class BtnRed extends React.Component //ความหมายไม่ชัดเจน
    resServer = () => ... //รับอะไรมาจาก server แก้เป็น getTimeNowFromServer
    get_Time, Gettime_From_server, _getTime //ยังไม่ต้องใช้ดีกว่า
```
    
## Function
  - สร้าง One Function Single Responsibility หรือ หนึ่งฟังก์ชั่นควรทำได้แค่ Job เดียว เพื่อให้ฟังก์ชั่นหลักใช้งาน
  - ใช้ Arrow Functions แล้วสั่งให้ฟังก์ชั่น ทำงานใน React LifeCycle หรือ Action ของฟังก์ชั่นต่างๆเท่านั้น อย่า void ใน class
  - หลีกเลี่ยงการใช้งานการวนลูป array ให้ใช้การ Filter()เพื่อหา map()เพื่อloop ข้อมูล แทน
  - ควรคอมเม้น หน้าที่ และ บอกค่าพารามิเตอร์ที่จะ return ของฟังก์ชั่นหลักไว้ด้วย ควรใช้ lowcase ทั้งหมด
  - console.log ใช้งานเสร็จแล้ว ลบหรือ คอมเม้นด้วย
  
```javascript
    // รูปแบบ arrow ฟังก์ชั่น
    myFunctionsAwsome = () => {
      //..
    }
    
    // ควรสร้างฟังก์ชั่นที่ทำได้แค่ Job เดียว เพื่อให้ฟังก์ชั่นหลักใช้งาน ใช้ซ้ำ และง่ายต่อการทำ Testing
    
    /*
     * no returns value
     * check user sesstion conditions
     * กรณีคอมเม้นหลายบรรทัด ให้ใช้ * บรรทัดเดียวใช้ //
     */
     
    signIn = asyn() => {
      if (this.checkSesstion() == null){
        // create new sesstion
        await this.getUserByUserName(id)
        await this.createSesstion(userById)
      } else if (this.checkUserExpire() < 90){
        // login sucessfuly
        this.props.goToHomePage()
      } else {
        // expire sesstion
        this.removeSesstion()
      }
    }
    createSesstion = (userById) => {}
    checkSesstion = () => {}
    removeSesstion = () => {}
    checkUserExpire = () => {}
    getUserByUserName = (id) => {}
    getTimeNowFromServer = () => {}
    
    // หลีกเลี่ยงการใช้งานการวนลูป array   
    this.props.tableList.filter(x => x.number === 0 ) //filter หา
    this.state.tableList.map((data) => { ... }) //loop ข้อมูล
    
```
  
## Workspace

 - **การแบ่งโฟลเดอร์**
  - คอมโพเน้นไฟลล์ที่เกี่ยวกับเรื่อง sesstion ให้ไว้ในโฟลวเดอร์ Authen
  - คอมโพเน้นหลัก(Container Component) ที่เกี่ยวกับแอพพลิเคชั่น ให้ไว้ใน `Hive/Component/...` โดยแบ่งตาม Routing ได้แก่ `Owner, Order, Payment, Setting, Market` โดยให้ตั้งชื่อไฟล์ว่า `index.js` และตั้งชื่อคอมโพเน้นท์เป็นชื่อของโฟเดอร์แทน (เพราะสะดวกต่อการอิมพอร์ต เนื่องจากสามารถอิมพอร์ตจากชื่อโฟเดอร์ได้เลย)
  - คอมโพเน้นที่แสดงผล (Presentation Component) ให้ไว้ในโฟเดอร์ของแต่ละ Routing ได้เลย
 - **การบริหาร asset**
  - ให้สร้างโฟลเดอร์ใน `/asset` เช่น รูปถ่าย(images)
  - ให้สร้าง packate.json ในแต่ละโฟลเดอร์ที่ระบุถึง Namespace และสามารถนำไปใช้ในไฟลล์ต่างๆได้ *ตัวอย่าง @img/test.jpg*
  
```javascript

    // ตั้งชื่อไฟล์ว่า index.jsx และตั้งชื่อคอมโพเน้นท์เป็นชื่อของโฟเดอร์แทน
    import Footer from './Footer'; // ระบบจะทำการอิมพอร์ตไฟล์ ./Footer/index.js ให้อัตโนมัติ
    
    // รูปแบบการแบ่งโฟลล์เดอร์
    | react
    | -- Hive
    | ---- index.js // เป็น config file ของ Routing Tab {Owner, Order, Payment, Setting, Market}
    | ---- Component
    | ------- Owner
    | ---------- Report
    | ------------- index.js // เป็น container ของ report
    | ------------- ReportDailySale.js
    | ------------- ReportListBook.js
    | ------------- ...
    | ---------- Branch
    | ------------- index.js // เป็น container ของ branch
    | ------------- BranchBox.js
    | ------------- BranchPromoBox.js
    | ------------- ...
    | ---------- index.js  // มี react naivgation config file ของ Routing Stack {Report,Branch}
    | ------- Order
    | ---------- index.js
    | ---------- ModalsSplitTable.js
    | ---------- ModalsMigrateTable.js
    | ---------- ModalsMergeTable.js
    | ---------- ModalsTableSetting.js
    | ---------- ...
    | ------- Payment
    | ------- Setting
    | ------- Market
    | ---- Warpper 
    | ------- Header.js
    | ------- TabBar.js
    | -- Authen
    | -- index.js // เป็น container สำหรับแบ่ง sesstions
    
```
## Written Style
  - การเรียกใช้ออปเจ็ค ให้ทำ object destruction หรือ ย่อรูปออปเจ็คเพื่อง่ายต่อการใช้งาน
  - การจัดรูปแบบโค้ดใน JSX นั้นให้ทำตามกฎของ Eslint ย่อหน้าให้ตรงกันในฟังก์ชั่น หรือ อิเลเม้นต์ที่อยู่ในระดับเดียวกัน
  - ใช้เขี้ยวคู่ (Double quotes) "" สำหรับค่าที่เป็น string เสมอ มีผลต่อการเว้นช่องว่าง แต่สำหรับโค้ดจาวาสคริปต์ทั่วไปให้ใช้เขี้ยวเดี่ยว (Single quotes) '' มีผลต่อการคำนวน
  - เว้นวรรคระหว่างฟังก์ชั่น เพียงครั้งเดียว
  ```javascript
  // การทำ object destruction
  let { title, desc, type } = action
  let { name, sName, address } = this.state
  let { name, sName, address } = this.props
  
  test = (value) => { value.well }  //ยังไม่ทำ
  test = ({value}) => { well }  //ทำแล้ว
  
  
 // การจัดรูปแบบโค้ดใน JSX
  <Foo
    superLongParam="bar"
    anotherSuperLongParam="baz"
  /> // ขึ้นบรรทัดใหม่ให้ตัวปิดและย่อหน้าตรงกัน พรอพเพอร์ตี้เข้าไป 1 ย่อหน้า
  
  <Foo bar="bar" /> // ถ้าพรอพเพอร์ตี้ไม่ยาวเกินไปสามารถใส่ไว้ในบรรทัดเดียวกันได้
  
  <Foo>
    //.
  </Foo> // อิเลเม้นต์ที่อยู่ในระดับเดียวกัน
  
  
 // การใช้ Quotes
  <Foo bar="bar foo 01234" />// "" สำหรับค่าที่เป็น string
  <Foo style={{ left: '20' }} /> '' สำหรับค่าที่เป็น script
  ```
## Dependency
  
  - ให้ใช้ dependency
  - เมื่อเพิ่ม Library ใดๆให้เพิ่มเข้าไปที่ dependency ด้วย `npm i ...... --save`
  - เมื่อเพิ่ม Library ใดๆให้ใช้จาก node_modules เท่านั้น
  
 > version hive-app-phase1-develop-1
