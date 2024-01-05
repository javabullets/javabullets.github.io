---
id: 1294
title: 'Java8 &#8211; Date and Time examples'
date: '2017-02-28T08:29:25+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=1294'
permalink: /java8-date-and-time-examples/
publicize_twitter_user:
    - glenwareltd
thrive_post_fonts:
    - '[]'
categories:
    - Java
    - Java8
tags:
    - date
    - java8
    - time
---

This post continues my look at Java8 features with the date API, including examples.

Before Java8 we had two date implementations – java.util.Date and java.util.Calendar. These implementations had numerous issues which Java8 sought to resolve. This article considers these issues, as well as providing a examples using the java.time API.

## Issues

- API Design – java.util.Date and java.util.Calendar both had issues like months starting from 0, or in the case of Date year starting from 1900. Date also represented a point in time, seconds from the epoch, while its toString method included a TimeZone. There was also no single class representing Time or Date
- Thread-safety – DateFormat not thread safe

De-facto standards – [Joda-Time](https://www.javabullets.com/2014/02/24/joda-time/) evolved as a defacto standard for Java dates. It is to Java8’s credit that they developed the Java8 JSR-310 API’s with the main joda-time developer Stephen Colebourne.

## New Features

- New package – java.time.\*
- Core Classes – LocalDate, LocalTime, LocalDateTime, ZonedDateTime, Period, Duration, Instant
- Immutable
- Factory methods

## Examples

```

import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.time.Duration;
import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.LocalTime;
import java.time.Period;
import java.time.ZoneId;
import java.time.ZonedDateTime;
import java.time.format.DateTimeFormatter;
import java.time.temporal.TemporalAdjuster;
import java.time.temporal.TemporalAdjusters;
import java.util.Calendar;
import java.util.Date;
import java.util.GregorianCalendar;

public class DateTimeExamples {

	public static void main(String[] args) {
		preJava8DateAndCalendar();
		java8LocalDateAndLocalTimeExamples();
		java8LocalDateTimeExamples();
		java8ZonedDateTime();
		java8CustomAdjuster();
		java8PeriodOrDuration();
	}

	public static void preJava8DateAndCalendar() {
		System.out.println("\npreJava8DateAndCalendar\n");

		Date today = new Date();
		System.out.println("Note the time includes the default timezone - " + today.toString());

		Date twentySevenFeb2017Date = new Date(117, 1, 27);
		System.out.println("Now deprecated new Date(day, month, year) - but note month starts at zero, and year 1900 - " + twentySevenFeb2017Date);

		Calendar twentySevenFeb2017Calendar = new GregorianCalendar(2017,1,27);
		System.out.println("Calendar - month starts at zero, and but year fixed - " + twentySevenFeb2017Calendar.getTime());

		DateFormat ddMMyyySDF = new SimpleDateFormat("dd/MM/yyyy");
		System.out.println("DateFormat not ThreadSafe - " + ddMMyyySDF.format(twentySevenFeb2017Date));
	}

	public static void java8LocalDateAndLocalTimeExamples() {
		// Date and Time split up - LocalDate, LocalTime
		// Called through factory methods of, now, parse
		// Immutable
		// No TimeZone
		System.out.println("\njava8LocalDateAndLocalTimeExamples\n");

		LocalDate currentLocalDate = LocalDate.now();
		System.out.println("currentLocalDate - yyyy-MM-dd - " + currentLocalDate);

		// Month now not based on 0, and year not based on 1900
		LocalDate twentySevenFeb2017LocalDate = LocalDate.of(2017, 2, 27);
		System.out.println("twentySevenFeb2017LocalDate - yyyy-MM-dd - " + twentySevenFeb2017LocalDate);

		twentySevenFeb2017LocalDate = twentySevenFeb2017LocalDate.withYear(2017).withMonth(12).withDayOfMonth(25);
		System.out.println("twentySevenFeb2017LocalDate - with -  " + twentySevenFeb2017LocalDate);

		LocalDate parseTwentySevenFeb2017LocalDate = LocalDate.parse("2017-02-27", DateTimeFormatter.ofPattern("yyyy-MM-dd"));
		System.out.println("parseTwentySevenFeb2017LocalDate - pattern - yyyy-MM-dd - " + parseTwentySevenFeb2017LocalDate);

		// increment using plus, decrement using minus
		twentySevenFeb2017LocalDate = twentySevenFeb2017LocalDate.plusDays(1);
		System.out.println("twentySevenFeb2017LocalDate - immutable - " + twentySevenFeb2017LocalDate);

		// Time with no date
		LocalTime currentLocalTime = LocalTime.now();
		System.out.println("currentLocalTime - yyyy-MM-dd - " + currentLocalTime);

		LocalTime parseLocalTime = LocalTime.parse("13:44");
		System.out.println("parseLocalTime - " + parseLocalTime);

		parseLocalTime = LocalTime.parse("13:44:25");
		System.out.println("parseLocalTime - immutable - " + parseLocalTime);

	}

	public static void java8LocalDateTimeExamples() {
		// Combines LocalDate and LocalTime
		// Called through factory methods of, now, parse
		// Immutable
		// No TimeZone
		System.out.println("\njava8LocalDateTimeExamples\n");

		LocalDateTime currentLocalDateTime = LocalDateTime.now();
		System.out.println("currentLocalDateTime " + currentLocalDateTime);		

		currentLocalDateTime = LocalDateTime.parse("2019-06-21T23:53:00.123");
		System.out.println("currentLocalDateTime - parse -  " + currentLocalDateTime);

		currentLocalDateTime = currentLocalDateTime.minusYears(10);
		System.out.println("currentLocalDateTime - minus 10 years -  " + currentLocalDateTime);
	}

	public static void java8ZonedDateTime() {
		System.out.println("\njava8ZonedDateTime\n");

		// Associate DateTime with TimeZone
		ZonedDateTime zonedDateTime = ZonedDateTime.of(LocalDateTime.now(), ZoneId.systemDefault());
		System.out.println("zonedDateTime - " + zonedDateTime);

		ZoneId australiaSydneyZoneId = ZoneId.of("Australia/Sydney");
		ZonedDateTime australiaSydneyZonedDateTime = ZonedDateTime.of(LocalDateTime.now(), australiaSydneyZoneId);
		System.out.println("australiaSydneyZonedDateTime - " + australiaSydneyZonedDateTime);
	}	

	public static void java8CustomAdjuster() {
		System.out.println("\njava8CustomAdjuster\n");
		TemporalAdjuster dueDateAdjuster = TemporalAdjusters.ofDateAdjuster((LocalDate localDate) -&amp;gt; localDate.plusWeeks(40));

		LocalDate startLocalDate = LocalDate.now();
		System.out.println("Due Date - " + startLocalDate.with(dueDateAdjuster));
	}

	public static void java8PeriodOrDuration() {
		System.out.println("\njava8DurationPeriodAndInstance\n");
		// Period - Duration in day, weeks, month, years
		Period examplePeriod = Period.of(72,6,10);
		System.out.println("examplePeriod " + examplePeriod);

		LocalDate localDatePlusExamplePeriod = LocalDate.now().plus(examplePeriod);
		System.out.println("localDatePlusExamplePeriod " + localDatePlusExamplePeriod);

		// Duration - days, hours, minutes, seconds
		Duration exampleDuration = Duration.ofHours(5);
		System.out.println("exampleDuration " + exampleDuration);

		LocalTime exampleDurationLocalTime = LocalTime.now().plus(exampleDuration);
		System.out.println("exampleDurationLocalTime " + exampleDurationLocalTime);		

	}	

}
```