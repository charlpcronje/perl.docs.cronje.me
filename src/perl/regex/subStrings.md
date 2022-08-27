---
title: Sub-String Matching | CRONje.ME
label: Sub-String Matching
order: 29
authors:
  - name: Charl Cronje
    email: charl@CRONje.ME
    link: https://blog.cronje.me
    avatar: https://assets.cronje.me/avatars/darker.jpg
---
# Sub-String Matching for [sample data](regexForSSHAgent.md)

[Source](https://www.geeksforgeeks.org/perl-substr-function/)

```perl
#!/usr/bin/perl
  
# String to be passed
$string = "GeeksForGeeks";
  
# Calling substr() to find string 
# without passing length
$sub_string1 = substr($string, 4);
  
# Printing the substring
print "Substring 1 : $sub_string1\n";
  
# Calling substr() to find the 
# substring of a fixed length
$sub_string2 = substr($string, 4, 5);
  
# Printing the substring

print "Substring 2 : $sub_string2 ";
```

## Output

```shell
Substring 1 : sForGeeks
Substring 2 : sForG 
```

### Example 2

```perl
#!/usr/bin/perl
  
# String to be passed
$string = "GeeksForGeeks";
  
# Calling substr() to find string 
# by passing negative index
$sub_string1 = substr($string, -4);
  
# Printing the substring
print "Substring 1 : $sub_string1\n";
  
# Calling substr() to find the 
# substring by passing negative length
$sub_string2 = substr($string, 4, -2);
  
# Printing the substring
print "Substring 2 : $sub_string2 ";
```

## Output

```perl
Substring 1 : eeks
Substring 2 : sForGee 
```
