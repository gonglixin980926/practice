# 正则匹配：
    *注：大写字符代表相反得意思*  
    \d 匹配数字  
    \w 字母 数字 下划线  
    . 匹配除换行以外的任何字符  
    + 表示一个或多个  
    \s 空白字符  
    * 0个或多个   
    \p 检测每一个字符的属性  
    ? 非贪婪模式 实现最少匹配 {}? *? +? 本身有0|1的意思
    $&  代表匹配到的正则
    &'  代表匹配到的后面
    &`  代表匹配到的前面
## 模式：
    s 忽略换行符   
    i 不区分大小写  
    g 表示全局匹配  
    gi 可以组合匹配 不分前后  
    m 每一行单独处理  
    u 代表宽字节   
    y 一直连续满足条件   
    `example:
        let str = 'Asd';
        str.replace(/a/gi,'@')`
    */p{sc=Han}/gu* 语言匹配
    
## 原子表 []：  
    [\s\S] || [\d\D] 匹配所有字符  
    []  字符出现在原子表中就匹配   
    原子表中代表一个字符  
    ^ 除了原子表中的字符                
    . + ( ) 在原子表里代表原含义    
## 原子组 ():  
    里面是一个整体  
    (com|org|cc|cn|net)    
    (\w+.)+ 原子组里的东西有一个或多个  
    (?:) 不记录当前原子组  
    `str.match(reg,(item,item1)=>{
        //每个item代表第几个原子组
    })
    
    str.matchAll()  
    //该方法为一个迭代器 用与全局匹配 低端浏览器没有此方法
    `
    (?<alias>)  &alias  

## exec 迭代器  
    需要使用全局模式匹配，不加全局就每次都是第一个位置  
    匹配每一个字符  
    `reg.lastIndex  //从0开始直到搜索完成
    while((res = reg.exec(str))){  
        res;  
    }  
    str.replace(reg,(p0,p1,p2)=>{  
        参数代表第几个原子组  
    })`  
  
## 多条正则  
    `let reg = [  
        /^[0-9a-z]{3,6}$/i,  
        /[A-Z]+/,  
        /[0-9]+/  
    ]`  
    *every 所有为真才返回真*  
    `let status =   reg.every((item)=>item.test(password))`
## matchAll
    对于不支持matchAll的浏览器需要写个polyfill  
    `
        String.prototype.matchAll = (reg)=>{
            let res = this.match(reg);
            if(res){
                let newStr = this.replace(reg,'^'.repeat(res[0].length));
                let match = newStr.matchAll(reg) || [];
            }
           
            return [res,...match]
        }
    `
    `
        //利用正则的方法
        String.prototype.search = (reg)=>{
            let ret = []
            while(res =reg.exec(this)){
                ret.push(res);

            }
            return ret
        }
    
    `
## 断言
    即正则表达式中的条件  不在匹配结果当中
    /reg(?=test)/   后面是什么   
    /(?<=test)reg/ 前面是什么  
    /(?![a-z])/i  后面不是字母
    /(?<!\d)*/   后面不是数字的