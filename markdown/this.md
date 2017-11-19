# ���this�����
������this,js�������͹�����һ�롣�����ܽ�һ��this���÷���
### һ����ȫ���������У����к����⣩���ֵ�this,ָȫ�ֶ���
`console.log(this) // window`
### �����ں����ڲ����ֵ�this,Ҫ��this���ڵĺ����ı����÷�ʽ
#### 1����ͨ�����б�ֱ�ӵ���,thisָȫ�ֶ���window
#### function��������
````function foo(){
    console.log(this)
}
foo()````
//function����������������
````var foo=function(){
    console.log(this)
}
foo()````
// ��ִ�к���
`(function(){console.log(this)})`
#### 2�����󷽷�����
`````var person={
    name:'person',
    run:function(){return this}
}
person.run()`````
//����this����ָ�������Ķ��󣬵��ṹ�Ƚϸ���ʱ�����磺
`````var animal={
name:'animal',
returnThis(){ return person.run()}
}
console.log(animal.returnThis())`````
// ʹ��this�ĺ�����person.run������this�󶨵�person��ֻҪ��ʹ��this���Ǹ�������
// �ܽ᣺����Ϊĳ������A�ķ�������ʱ��thisָ���������Ķ���A��������˭����˭ֱ�ӵ��ã�
// this��ָ��˭
// 3���¼��󶨣�
````var btn = document.querySelector("button")
btn.onclick = function () {
console.log(this) // btn
}````
// 4���¼�������
````var btn = document.querySelector("button")
btn.addEventListener('click', function () {
console.log(this) //btn
})````
#### 5���ڹ��캯�����߹��캯��ԭ�Ͷ�����thisָ���캯����ʵ��
��ʹ��newָ��window
`````function Person(name){
console.log(this)//window
this.name=name
}
Person("С��")`````
ʹ��new
``````function Person(name){
this.name=name
self = this
}
var people = new Person('xiaoming')
console.log(self === people)// true``````
// ����new�ı���thisָ�򣬽�this��windowָ��Person��ʵ������people��
// people�൱�ڸ����˹��캯����ֵ
// ����new ��������ú���ʱ���ú����ܻ᷵��һ������ͨ�����this����ָ���������
����һ���������:�����������ʽ�ķ���һ��object��Ķ������������շ��ص����Ǹ����������this
``````function Person2(name){
this.name=name
return{name:"xiaowang"}
}

var obj = new Person2('xiaoming')
console.log(obj.name)``````
#### 5��jquery��ajax
``````````$.ajax({
self: this,
type:"get",
url: url,
async:true,
success: function (res) {
console.log(this) // thisָ����$.ajxa()�еĶ���
console.log(self) // window
}
});``````````
//����˵��һ�£��������дΪ$.ajax��obj�� ��thisָ��obj,��obj��thisָ��window��
// ��Ϊ����success�����У�����obj�����Լ�������thisָ��obj

## �ı�this��ָ������
###  һ��  Function.prototype.call����Function.prototype.apply���Զ�̬�ı�this
//    ���ߵĹ�����ȫһ����ֻ�ǲ�����ͬ
//    call��һ��������thisҪָ��������ģ�������ŵĲ������̶������ǵ��ɺ����Ĳ������ݽ�ȥ
//    apply��һ������Ҳ��thisҪָ��������ģ�������һ��������������飬�����ÿһ���˳�򵱳��Ǻ����Ĳ������ݽ�ȥ��Ҳ��������������������Ǻ�����arguments
      ````var obj1={
          name:'С��',
          fruit:'apple'
      }````
      ````var obj2={
          name:'С��',
          toy:'car'
      }````
      ````var getToy=function(x,y){
          console.log(x,y)
          console.log(this.toy)
      }````
      ````var getFruit=function(x,y){
          console.log(x,y)
          console.log(this.fruit)
      }````
//        getToy("x","y")
       `` getFruit.call(obj1,"x","y")
        getToy.apply(obj2,["x","y"])``

// ΪʲôҪ�ı�����������?��һ������û��ĳ��������ʱ�򣬶���һ�����������������
// ���ʱ��Ϳ���ͨ���ı���������ʹ���Լ�û�еķ����ˡ�
// ��ʲôʱ����callʲôʱ����apply�������������ǹ̶������ʱ�������call��������������ȷ��������apply
//ʹ�ú�����apply��call��������ʱ��thisָ��һ������B��
//    func.apply(B, [m, n, ...]);
//    func.call(B, m, n, ...);

### ����Object.prototype.bind��������ͨ��һ���º������ṩ���õİ󶨣����Ḳ��call��aplply�İ󶨡�
    ```function returnThis(){
        return this
    }```
    ``````var s1={name:'s1'}
    var s2={name:'s2'}
    var s1returnThis = returnThis.bind(s1)
    console.log(s1returnThis())
    s1returnThis.call(s2)
    console.log(s1returnThis())``````
### ����һ���Ƚ����׺��ԵĻ��this�ĵط�����new��������newһ������ʱ���ͻ��Զ���this�����¶����ϣ�Ȼ���ٵ������������
// ���Ḳ��bind�İ󶨡�
    function showThis () {
        console.log(this)
    }
    showThis() // window
    new showThis() // showThis
    var boss1 = { name: 'boss1' }
    showThis.call(boss1) // boss1
    new showThis.call(boss1) // TypeError
    var boss1showThis = showThis.bind(boss1)
    boss1showThis() // boss1
    new boss1showThis() // showThis
### �ġ���ͷ�������󶨽���������
//  ���ڼ�ͷ������ֻҪ���������ﴴ��
    ````function callback (cb) {
        cb()
    }
    callback(() => { console.log(this) }) // window````
    ``````var boss1 = {
        name: 'boss1',
        callback: callback,
        callback2 () {
        callback(() => { console.log(this) })
    }``````
    ``boss1.callback(() => { console.log(this) }) // still window
    boss1.callback2(() => { console.log(this) }) // boss1``