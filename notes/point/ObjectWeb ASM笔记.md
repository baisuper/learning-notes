# 写在前头
如果你想快速了解如何试用asm
# 基础知识


# 分析一个类(ClassVisitor)
假设我们要读取一个类并且打印它的内容，首先我们定一个ClassPrinter的类来继承ClassVisitor。
```java
public class ClassPrinter extends ClassVisitor {
    public ClassPrinter() {
        super(ASM4);
    }

    @Override
    public void visit(int version, int access, String name,
                      String signature, String superName, String[] interfaces) {
        System.out.println(name + " extends " + superName + " {");
    }

    @Override
    public void visitSource(String source, String debug) {
    }

    @Override
    public void visitOuterClass(String owner, String name, String desc) {
    }

    @Override
    public AnnotationVisitor visitAnnotation(String desc,
                                             boolean visible) {
        return null;
    }

    @Override
    public void visitAttribute(Attribute attr) {
    }

    @Override
    public void visitInnerClass(String name, String outerName,
                                String innerName, int access) {
    }

    @Override
    public FieldVisitor visitField(int access, String name, String desc,
                                   String signature, Object value) {
        System.out.println(" " + desc + " " + name);
        return null;
    }

    @Override
    public MethodVisitor visitMethod(int access, String name, String desc, String signature, String[] exceptions) {
        System.out.println(" " + name + desc);
        return null;
    }

    @Override
    public void visitEnd() {
        System.out.println("}");
    }

}
```
接下来调用我们定义的类
```java
ClassPrinter cp = new ClassPrinter();
ClassReader cr = new ClassReader("java.lang.Runnable");
cr.accept(cp, 0);
```

可以看到输出如下内容
![](https://oscimg.oschina.net/oscnet/up-bab041dbbeda3356cfb09db3221928db9ae.png)

从上面的代码可以看到，如果想要访问一个类的定义内容，主要是构建ClassReader，而构建ClassReader的方式有很多，它的构造函数如下
![](https://oscimg.oschina.net/oscnet/up-e95344060ebc553e6f2db3c2b01413485af.png)
比如我们想读取某个jar中的所有的类信息并且打印出来的话，可以通过InputStream来构建
```java
public void loadJarFromAbsolute(String path) throws IOException {
    JarFile jar = new JarFile(path);
    Enumeration<JarEntry> entryEnumeration = jar.entries();
    while (entryEnumeration.hasMoreElements()) {
        JarEntry entry = entryEnumeration.nextElement();
        String name = entry.getName();
        if (!entry.isDirectory() && name.endsWith(".class")) {
            ClassPrinter cp = new ClassPrinter();
            ClassReader cr = new ClassReader(jar.getInputStream(entry));
            cr.accept(cp, 0);
        }
    }
}
```


# 生成一个类