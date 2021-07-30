---
title: "Integrating Java with Bash pipes"
date: 2012-11-23T01:44:00+01:00
draft: false
---

For testing purpose we will try to develop a simple Java class to replicate the GNU `grep` behavior.
Lets take the following `grep` as the test baseline.

<!--more-->

```
$ cat /proc/cpuinfo | grep "model name"
model name      : Intel(R) Core(TM) i7-2640M CPU @ 2.80GHz
model name      : Intel(R) Core(TM) i7-2640M CPU @ 2.80GHz
model name      : Intel(R) Core(TM) i7-2640M CPU @ 2.80GHz
model name      : Intel(R) Core(TM) i7-2640M CPU @ 2.80GHz
```

`grep` receives a pattern and uses it to filter lines from `stdin` and place them into `stdout`.
Pretty straightforward, right? The following Java class tries to accomplish just that.

```
package me.defying.jgrep;
 
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.Reader;
 
public class JGrep {
 
    public static void main(String[] args) throws IOException {
        Reader reader = new InputStreamReader(System.in);
        BufferedReader in = new BufferedReader(reader);
 
        String line;
        while ((line = in.readLine()) != null) {
            if (args.length == 0 || line.contains(args[0])) {
                System.out.println(line);
            }
        }
    }
}
```

It is time to tryout our new Java `grep`, `jgrep` for friends.

```
$ javac me/defying/jgrep/JGrep.java
```
```
$ cat /proc/cpuinfo | java me.defying.jgrep.JGrep "model name"
model name      : Intel(R) Core(TM) i7-2640M CPU @ 2.80GHz
model name      : Intel(R) Core(TM) i7-2640M CPU @ 2.80GHz
model name      : Intel(R) Core(TM) i7-2640M CPU @ 2.80GHz
model name      : Intel(R) Core(TM) i7-2640M CPU @ 2.80GHz
```

Yey! Same output.

If the application we are developing is going to be used on a daily basis we need a more convenient way to execute it. For instance, we may create an alias in our `.bashrc` file or even create an executable script and put it in our `$PATH` environment variable.

```
$ alias jgrep="java me.defying.jgrep.JGrep"
$ cat /proc/cpuinfo | jgrep "model name"
model name      : Intel(R) Core(TM) i7-2640M CPU @ 2.80GHz
model name      : Intel(R) Core(TM) i7-2640M CPU @ 2.80GHz
model name      : Intel(R) Core(TM) i7-2640M CPU @ 2.80GHz
model name      : Intel(R) Core(TM) i7-2640M CPU @ 2.80GHz
```

To wrap things up, we may add new pipes to our `jgrep` output.

```
$ cat /proc/cpuinfo | jgrep "model name" | wc
      4      36     216
```