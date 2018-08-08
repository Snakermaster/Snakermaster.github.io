---
layout:post
title:一道面向对象的简单案例
date:2018-06-20
tag:博客
---


  在编程学习中,面向对象思维解决问题一直是很多初学者的难题，下面通过一道‘人开枪射击子弹的例题’来简单阐述一下面向对象的思维过程是怎样的，首先我们需要根据人，枪，子弹盒的属性以及其方法简单分析如下面注释所示：
"""
类名：人
属性：枪
方法：开火；装子弹

类名：枪
属性：弹夹
方法：射击

类名：弹夹
属性：子弹数量
方法：无

"""

def main():

    # 创建人类
    class Person(object):

        def __init__(self,gun):
            self.gun=gun

        def fire(self):
            self.gun.shoot()

        def fill_bullet(self):
            if self.gun.bulletbox.bullet < 5 :
                self.gun.bulletbox.bullet += 5
                print('现在有%d发子弹'%(self.gun.bulletbox.bullet))
            else:
                print('现在还不能加子弹')

    # 创建枪类

    class Gun(object):
        def __init__(self,bulletbox):
            self.bulletbox=bulletbox

        def shoot(self):
            if self.bulletbox.bullet == 0:
                print('没有子弹了！')
            else:
                self.bulletbox.bullet -=1
                print('还剩%d发子弹'%(self.bulletbox.bullet))

    # 创建子弹盒类

    class Bulletbox:
        def __init__(self,bulletcount):
            self.bulletcount=bulletcount


    # 创建子弹对象

    bulletbox = Bulletbox(5)

    # 创建手枪对象

    gun = Gun(bulletbox)

    # 创建人对象

    per = Person(gun)

    # 通过对象调用开火方法

    per.fire()
    per.fire()
    per.fire()
    per.fire()
    per.fire()
    per.fire()

    # 通过对象加子弹操作

    per.fill_bullet()
    per.fill_bullet()




if __name__=='__main__':

    main()




