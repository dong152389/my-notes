# Thymeleaf 内置对象

## `#dates`

```text
/*
* ======================================================================
* See javadoc API for class org.thymeleaf.expression.Dates
* ======================================================================
*/
/*
* Format date with the standard locale format
* Also works with arrays, lists or sets
*/
${#dates.format(date)}
${#dates.arrayFormat(datesArray)}
${#dates.listFormat(datesList)}
${#dates.setFormat(datesSet)}
/*
* Format date with the ISO8601 format
* Also works with arrays, lists or sets
*/
${#dates.formatISO(date)}
${#dates.arrayFormatISO(datesArray)}
${#dates.listFormatISO(datesList)}
${#dates.setFormatISO(datesSet)}
/*
* Format date with the specified pattern
* Also works with arrays, lists or sets
*/
${#dates.format(date, 'dd/MMM/yyyy HH:mm')}
${#dates.arrayFormat(datesArray, 'dd/MMM/yyyy HH:mm')}
${#dates.listFormat(datesList, 'dd/MMM/yyyy HH:mm')}
${#dates.setFormat(datesSet, 'dd/MMM/yyyy HH:mm')}
/*
* Obtain date properties
* Also works with arrays, lists or sets
*/
${#dates.day(date)} // also arrayDay(...), listDay(...), etc.
${#dates.month(date)} // also arrayMonth(...), listMonth(...), etc.
${#dates.monthName(date)} // also arrayMonthName(...), listMonthName(...), etc.
${#dates.monthNameShort(date)} // also arrayMonthNameShort(...), listMonthNameShort(...), etc.
${#dates.year(date)} // also arrayYear(...), listYear(...), etc.
${#dates.dayOfWeek(date)} // also arrayDayOfWeek(...), listDayOfWeek(...), etc.
${#dates.dayOfWeekName(date)} // also arrayDayOfWeekName(...), listDayOfWeekName(...), etc.
${#dates.dayOfWeekNameShort(date)} // also arrayDayOfWeekNameShort(...), listDayOfWeekNameShort(...), etc.
${#dates.hour(date)} // also arrayHour(...), listHour(...), etc.
${#dates.minute(date)} // also arrayMinute(...), listMinute(...), etc.
${#dates.second(date)} // also arraySecond(...), listSecond(...), etc.
${#dates.millisecond(date)} // also arrayMillisecond(...), listMillisecond(...), etc.
/*
* Create date (java.util.Date) objects from its components
*/
${#dates.create(year,month,day)}
${#dates.create(year,month,day,hour,minute)}
${#dates.create(year,month,day,hour,minute,second)}
${#dates.create(year,month,day,hour,minute,second)}
${#dates.create(year,month,day,hour,minute,second,millisecond)}
/*
* Create a date (java.util.Date) object for the current date and time
*/
${#dates.createNow()}
/*
* Create a date (java.util.Date) object for the current date (time set to 00:00)
*/
${#dates.createToday()}
```

## `#Calendars`

```text
/*
* ======================================================================
* See javadoc API for class org.thymeleaf.expression.Calendars
* ======================================================================
*/
/*
* Format calendar with the standard locale format
* Also works with arrays, lists or sets
*/
${#calendars.format(cal)}
${#calendars.arrayFormat(calArray)}
${#calendars.listFormat(calList)}
${#calendars.setFormat(calSet)}
/*
* Format calendar with the ISO8601 format
* Also works with arrays, lists or sets
*/
${#calendars.formatISO(cal)}
${#calendars.arrayFormatISO(calArray)}
${#calendars.listFormatISO(calList)}
${#calendars.setFormatISO(calSet)}
/*
* Format calendar with the specified pattern
* Also works with arrays, lists or sets
*/
${#calendars.format(cal, 'dd/MMM/yyyy HH:mm')}
${#calendars.arrayFormat(calArray, 'dd/MMM/yyyy HH:mm')}
${#calendars.listFormat(calList, 'dd/MMM/yyyy HH:mm')}
${#calendars.setFormat(calSet, 'dd/MMM/yyyy HH:mm')}
/*
* Obtain calendar properties
* Also works with arrays, lists or sets
*/
${#calendars.day(date)} // also arrayDay(...), listDay(...), etc.
${#calendars.month(date)} // also arrayMonth(...), listMonth(...), etc.
${#calendars.monthName(date)} // also arrayMonthName(...), listMonthName(...), etc.
${#calendars.monthNameShort(date)} // also arrayMonthNameShort(...), listMonthNameShort(...), etc.
${#calendars.year(date)} // also arrayYear(...), listYear(...), etc.
${#calendars.dayOfWeek(date)} // also arrayDayOfWeek(...), listDayOfWeek(...), etc.
${#calendars.dayOfWeekName(date)} // also arrayDayOfWeekName(...), listDayOfWeekName(...), etc.
${#calendars.dayOfWeekNameShort(date)} // also arrayDayOfWeekNameShort(...), listDayOfWeekNameShort(...), etc.
${#calendars.hour(date)} // also arrayHour(...), listHour(...), etc.
${#calendars.hour(date)} // also arrayHour(...), listHour(...), etc.
${#calendars.minute(date)} // also arrayMinute(...), listMinute(...), etc.
${#calendars.second(date)} // also arraySecond(...), listSecond(...), etc.
${#calendars.millisecond(date)} // also arrayMillisecond(...), listMillisecond(...), etc.
/*
* Create calendar (java.util.Calendar) objects from its components
*/
${#calendars.create(year,month,day)}
${#calendars.create(year,month,day,hour,minute)}
${#calendars.create(year,month,day,hour,minute,second)}
${#calendars.create(year,month,day,hour,minute,second,millisecond)}
/*
* Create a calendar (java.util.Calendar) object for the current date and time
*/
${#calendars.createNow()}
/*
* Create a calendar (java.util.Calendar) object for the current date (time set to 00:00)
*/
${#calendars.createToday()}
```

## `#numbers`

```text
/*
* ======================================================================
* See javadoc API for class org.thymeleaf.expression.Numbers
* ======================================================================
*/
/*
* ==========================
* Formatting integer numbers
* ==========================
*/
/*
* Set minimum integer digits.
* Also works with arrays, lists or sets
*/
${#numbers.formatInteger(num,3)}
${#numbers.arrayFormatInteger(numArray,3)}
${#numbers.listFormatInteger(numList,3)}
${#numbers.setFormatInteger(numSet,3)}
/*
* Set minimum integer digits and thousands separator:
* 'POINT', 'COMMA', 'WHITESPACE', 'NONE' or 'DEFAULT' (by locale).
* Also works with arrays, lists or sets
*/
${#numbers.formatInteger(num,3,'POINT')}
${#numbers.arrayFormatInteger(numArray,3,'POINT')}
${#numbers.listFormatInteger(numList,3,'POINT')}
${#numbers.setFormatInteger(numSet,3,'POINT')}
/*
* ==========================
* Formatting decimal numbers
* ==========================
*/
/*
* Set minimum integer digits and (exact) decimal digits.
* Also works with arrays, lists or sets
*/
${#numbers.formatDecimal(num,3,2)}
${#numbers.arrayFormatDecimal(numArray,3,2)}
${#numbers.listFormatDecimal(numList,3,2)}
${#numbers.setFormatDecimal(numSet,3,2)}
/*
* Set minimum integer digits and (exact) decimal digits, and also decimal separator.
* Also works with arrays, lists or sets
*/
${#numbers.formatDecimal(num,3,2,'COMMA')}
${#numbers.arrayFormatDecimal(numArray,3,2,'COMMA')}
${#numbers.listFormatDecimal(numList,3,2,'COMMA')}
${#numbers.setFormatDecimal(numSet,3,2,'COMMA')}
/*
* Set minimum integer digits and (exact) decimal digits, and also thousands and
* decimal separator.
* Also works with arrays, lists or sets
*/
${#numbers.formatDecimal(num,3,'POINT',2,'COMMA')}
${#numbers.arrayFormatDecimal(numArray,3,'POINT',2,'COMMA')}
${#numbers.listFormatDecimal(numList,3,'POINT',2,'COMMA')}
${#numbers.setFormatDecimal(numSet,3,'POINT',2,'COMMA')}
/*
* ==========================
* Utility methods
* ==========================
*/
/*
* Create a sequence (array) of integer numbers going
* from x to y
*/
${#numbers.sequence(from,to)}
${#numbers.sequence(from,to,step)}
```

## `#strings`

```text
/*
* ======================================================================
* See javadoc API for class org.thymeleaf.expression.Strings
* ======================================================================
*/
/*
* Null-safe toString()
*/
${#strings.toString(obj)} // also array*, list* and set*
/*
* Check whether a String is empty (or null). Performs a trim() operation before check
* Also works with arrays, lists or sets
* * Also works with arrays, lists or sets
*/
${#strings.isEmpty(name)}
${#strings.arrayIsEmpty(nameArr)}
${#strings.listIsEmpty(nameList)}
${#strings.setIsEmpty(nameSet)}
/*
* Perform an 'isEmpty()' check on a string and return it if false, defaulting to
* another specified string if true.
* Also works with arrays, lists or sets
*/
${#strings.defaultString(text,default)}
${#strings.arrayDefaultString(textArr,default)}
${#strings.listDefaultString(textList,default)}
${#strings.setDefaultString(textSet,default)}
/*
* Check whether a fragment is contained in a String
* Also works with arrays, lists or sets
*/
${#strings.contains(name,'ez')} // also array*, list* and set*
${#strings.containsIgnoreCase(name,'ez')} // also array*, list* and set*
/*
* Check whether a String starts or ends with a fragment
* Also works with arrays, lists or sets
*/
${#strings.startsWith(name,'Don')} // also array*, list* and set*
${#strings.endsWith(name,endingFragment)} // also array*, list* and set*
/*
* Substring-related operations
* Also works with arrays, lists or sets
*/
${#strings.indexOf(name,frag)} // also array*, list* and set*
${#strings.substring(name,3,5)} // also array*, list* and set*
${#strings.substringAfter(name,prefix)} // also array*, list* and set*
${#strings.substringBefore(name,suffix)} // also array*, list* and set*
${#strings.replace(name,'las','ler')} // also array*, list* and set*
/*
* Append and prepend
* Also works with arrays, lists or sets
*/
${#strings.prepend(str,prefix)} // also array*, list* and set*
${#strings.append(str,suffix)} // also array*, list* and set*
/*
* Change case
* Also works with arrays, lists or sets
*/
${#strings.toUpperCase(name)} // also array*, list* and set*
${#strings.toLowerCase(name)} // also array*, list* and set*
/*
* Split and join
*/
${#strings.arrayJoin(namesArray,',')}
${#strings.listJoin(namesList,',')}
${#strings.setJoin(namesSet,',')}
${#strings.arraySplit(namesStr,',')} // returns String[]
${#strings.listSplit(namesStr,',')} // returns List<String>
${#strings.setSplit(namesStr,',')} // returns Set<String>
/*
* Trim
* Also works with arrays, lists or sets
*/
${#strings.trim(str)} // also array*, list* and set*
/*
* Compute length
* Also works with arrays, lists or sets
*/
${#strings.length(str)} // also array*, list* and set*
/*
* Abbreviate text making it have a maximum size of n. If text is bigger, it
* will be clipped and finished in "..."
* Also works with arrays, lists or sets
*/
${#strings.abbreviate(str,10)} // also array*, list* and set*
/*
* Convert the first character to upper-case (and vice-versa)
*/
${#strings.capitalize(str)} // also array*, list* and set*
${#strings.unCapitalize(str)} // also array*, list* and set*
/*
* Convert the first character of every word to upper-case
*/
${#strings.capitalizeWords(str)} // also array*, list* and set*
${#strings.capitalizeWords(str,delimiters)} // also array*, list* and set*
/*
* Escape the string
*/
${#strings.escapeXml(str)} // also array*, list* and set*
${#strings.escapeJava(str)} // also array*, list* and set*
${#strings.escapeJavaScript(str)} // also array*, list* and set*
${#strings.unescapeJava(str)} // also array*, list* and set*
${#strings.unescapeJavaScript(str)} // also array*, list* and set*
/*
* Null-safe comparison and concatenation
*/
${#strings.equals(first, second)}
${#strings.equalsIgnoreCase(first, second)}
${#strings.concat(values...)}
${#strings.concatReplaceNulls(nullValue, values...)}
/*
* Random
*/
${#strings.randomAlphanumeric(count)}
```

## `#objects`

```text
/*
* ======================================================================
* See javadoc API for class org.thymeleaf.expression.Objects
* ======================================================================
*/
/*
* Return obj if it is not null, and default otherwise
* Also works with arrays, lists or sets
*/
${#objects.nullSafe(obj,default)}
${#objects.arrayNullSafe(objArray,default)}
${#objects.listNullSafe(objList,default)}
${#objects.setNullSafe(objSet,default)}
```

## `#bools`

```text
/*
* ======================================================================
* See javadoc API for class org.thymeleaf.expression.Bools
* ======================================================================
*/
/*
* Evaluate a condition in the same way that it would be evaluated in a th:if tag
* (see conditional evaluation chapter afterwards).
* Also works with arrays, lists or sets
*/
${#bools.isTrue(obj)}
${#bools.arrayIsTrue(objArray)}
${#bools.listIsTrue(objList)}
${#bools.setIsTrue(objSet)}
/*
* Evaluate with negation
* Also works with arrays, lists or sets
*/
${#bools.isFalse(cond)}
${#bools.arrayIsFalse(condArray)}
${#bools.listIsFalse(condList)}
${#bools.setIsFalse(condSet)}
/*
* Evaluate and apply AND operator
* Receive an array, a list or a set as parameter
*/
${#bools.arrayAnd(condArray)}
${#bools.listAnd(condList)}
${#bools.setAnd(condSet)}
/*
* Evaluate and apply OR operator
* Receive an array, a list or a set as parameter
*/
${#bools.arrayOr(condArray)}
${#bools.listOr(condList)}
${#bools.setOr(condSet)}
```

## `#arrays`

```text
/*
* ======================================================================
* See javadoc API for class org.thymeleaf.expression.Arrays
* ======================================================================
*/
/*
* Converts to array, trying to infer array component class.
* Note that if resulting array is empty, or if the elements
* of the target object are not all of the same class,
* this method will return Object[].
*/
${#arrays.toArray(object)}
/*
* Convert to arrays of the specified component class.
*/
${#arrays.toStringArray(object)}
${#arrays.toIntegerArray(object)}
${#arrays.toLongArray(object)}
${#arrays.toDoubleArray(object)}
${#arrays.toFloatArray(object)}
${#arrays.toBooleanArray(object)}
/*
* Compute length
*/
${#arrays.length(array)}
/*
* Check whether array is empty
*/
${#arrays.isEmpty(array)}
/*
* Check if element or elements are contained in array
*/
${#arrays.contains(array, element)}
${#arrays.containsAll(array, elements)}
```

## `#lists`

```text
/*
* ======================================================================
* See javadoc API for class org.thymeleaf.expression.Lists
* ======================================================================
*/
/*
* Converts to list
*/
${#lists.toList(object)}
/*
* Compute size
*/
${#lists.size(list)}
/*
* Check whether list is empty
*/
${#lists.isEmpty(list)}
/*
* Check if element or elements are contained in list
*/
${#lists.contains(list, element)}
${#lists.containsAll(list, elements)}
/*
* Sort a copy of the given list. The members of the list must implement
* comparable or you must define a comparator.
*/
${#lists.sort(list)}
${#lists.sort(list, comparator)}
```

## `#sets`

```text
/*
* ======================================================================
* See javadoc API for class org.thymeleaf.expression.Sets
* ======================================================================
*/
/*
* Converts to set
*/
${#sets.toSet(object)}
/*
* Compute size
*/
${#sets.size(set)}
/*
* Check whether set is empty
*/
${#sets.isEmpty(set)}
/*
* Check if element or elements are contained in set
*/
${#sets.contains(set, element)}
${#sets.containsAll(set, elements)}
```

## `#maps`

```text
/*
* ======================================================================
* See javadoc API for class org.thymeleaf.expression.Maps
* ======================================================================
*/
/*
* Compute size
*/
${#maps.size(map)}
/*
* Check whether map is empty
*/
${#maps.isEmpty(map)}
/*
* Check if key/s or value/s are contained in maps
*/
${#maps.containsKey(map, key)}
${#maps.containsAllKeys(map, keys)}
${#maps.containsValue(map, value)}
${#maps.containsAllValues(map, value)}
```

## `#aggregates`

```text
/*
* ======================================================================
* See javadoc API for class org.thymeleaf.expression.Aggregates
* ======================================================================
*/
/*
* Compute sum. Returns null if array or collection is empty
*/
${#aggregates.sum(array)}
${#aggregates.sum(collection)}
/*
* Compute average. Returns null if array or collection is empty
*/
${#aggregates.avg(array)}
${#aggregates.avg(collection)}
```

## `#messages`

```text
/*
* ======================================================================
* See javadoc API for class org.thymeleaf.expression.Messages
* ======================================================================
*/
/*
* Obtain externalized messages. Can receive a single key, a key plus arguments,
* or an array/list/set of keys (in which case it will return an array/list/set of
* externalized messages).
* If a message is not found, a default message (like '??msgKey??') is returned.
*/
${#messages.msg('msgKey')}
${#messages.msg('msgKey', param1)}
${#messages.msg('msgKey', param1, param2)}
${#messages.msg('msgKey', param1, param2, param3)}
${#messages.msgWithParams('msgKey', new Object[] {param1, param2, param3, param4})}
${#messages.arrayMsg(messageKeyArray)}
${#messages.listMsg(messageKeyList)}
${#messages.setMsg(messageKeySet)}
/*
* Obtain externalized messages or null. Null is returned instead of a default
* message if a message for the specified key is not found.
*/
${#messages.msgOrNull('msgKey')}
${#messages.msgOrNull('msgKey', param1)}
${#messages.msgOrNull('msgKey', param1, param2)}
${#messages.msgOrNull('msgKey', param1, param2, param3)}
${#messages.msgOrNullWithParams('msgKey', new Object[] {param1, param2, param3, param4})}
${#messages.arrayMsgOrNull(messageKeyArray)}
${#messages.listMsgOrNull(messageKeyList)}
${#messages.setMsgOrNull(messageKeySet)}
```

## `#ids`

```text
/*
* ======================================================================
* See javadoc API for class org.thymeleaf.expression.Ids
* ======================================================================
*/
/*
* Normally used in th:id attributes, for appending a counter to the id attribute value
* so that it remains unique even when involved in an iteration process.
*/
${#ids.seq('someId')}
/*
* Normally used in th:for attributes in <label> tags, so that these labels can refer to Ids
* generated by means if the #ids.seq(...) function.
*
* Depending on whether the <label> goes before or after the element with the #ids.seq(...)
* function, the "next" (label goes before "seq") or the "prev" function (label goes after
* "seq") function should be called.
*/
${#ids.next('someId')}
${#ids.prev('someId')}
```