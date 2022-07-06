装箱就是 自动将基本数据类型转换为包装类型；

拆箱就是 自动将包装类型转换为基本数据类型；

在定义变量的时候，比如Integer num = 1;就会自动装箱成Integer对象操作，int num2 = num;就会进行自动拆箱操作

在比较的时候，也会会发生拆箱和装箱操作

无论如何，Integer与new Integer不会相等。不会经历拆箱过程
两个都是非new出来的Integer，如果数在-128到127之间，则是true,否则为false
两个都是new出来的,都为false
int和Integer或者new Integer比较，都为true，因为会把Integer自动拆箱为int再去比
