声明native方法

```Android

public native String stringFromJni();

```

load the shared Libraries

```android

static{

    System.loadLibrary("hello-jni");

}

```

### java中有2种方法:instance method and static method

instance method 时,Native instance method get the instance reference as their second parameter as a jobject value.as shown below.

```c

JNIEXPORT jstring JNICALL Java_com_example_hellojni_HelloJni_stringFromJNI (

  JNIEnv * env,jobject thiz);

```

由于静态方法不需要绑定对象,Native methods get the class reference as a jclass value,as shown below.

```c

JNIEXPORT jstring JNICALL Java_com_example_hellojni_HelloJni_stringFromJNI (

  JNIEnv * env,jclass clazz);

```

### java中有2种类型的数据:Primitive(原始) types and Reference types.

***Primitive*** types are directly mapped to c/c++ equivalents.The jni uses type definitions to make this mapping transparent to developers.

| java | jni |

| -- | -- |

| Boolean | Jboolean |

| Byte | Jbyte |

| Char | Jchar |

| Short | Jshort |

| Long | Jlong |

| Float | Jfloat |

| String | jstring |

| Object[] | jobjectArray |

| boolean[]| jbooleanArray |

| byte[] | jbyteArray |

| char[] | jcharArray |

| short[] | jshortArray |

| int[] | jintArray |

| long[] | jlongArray |

| float[] | jfloatArray |

| double[] | jdoubleArray |

| other arrays | Jarray |

****Reference**** types are passed as opaque(不透明) references to the native code rather than native data types,and they cannot be consumed and modified directly.JNI provides a set of APIs for interacting with these reference types.These APIs are provided to native function through the JNIEnv interface pointer.next step ,we will briefly explore these APIs pertinent(有关的) to the following types and components:

***Strings,Arrays,NIO Buffers,Fields,Methods***

####String Operations

Java strings are handled by the JNI as reference types. These reference types are not directly usable as native C strings.JNI provides the necessary functions to convert these java string reference to C strings and back.Since java string objects are immutable ,JNI does not provide any function to modify the content of an existing java string.****JNI supports both Unicode and UTF-8 encoded strings,and it provides two sets of functions through the JNIEnv interface pointer to handle each of these string encodings.

****New String :  convert to java string****

New string instances can be constructed from the native code by using the functions NewString for Unicode strings and NewStringUTF for UTF-8 strings.these functions take a c string and returns a java string reference type,a jstring value.

```c

jstring javaString;

javaString = (*env)->NewStringUTF(env,"hello");

```

****convert to c string ****

