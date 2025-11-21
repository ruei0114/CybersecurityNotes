#shell
### Shebang
- <font color = "c2e3f4">starting with</font>`#!`
- <font color = "c2e3f4">define the interpreter to execute the script</font>
```sh
#!/bin/bash
```

---

### Variables
```sh
# Defining the Interpreter 
#!/bin/bash
echo "Hey, what’s your name?"
read name
echo "Welcome, $name"
```

---

### Loops
```sh
# Defining the Interpreter 
#!/bin/bash
for i in {1..10};
do
echo $i
done
```

---

### Conditional Statements
```sh
# Defining the Interpreter 
#!/bin/bash
echo "Please enter your name first:"
read name
if [ "$name" = "Stewart" ]; then
        echo "Welcome Stewart! Here is the secret: THM_Script"
else
        echo "Sorry! You are not authorized to access the secret."
fi
```

