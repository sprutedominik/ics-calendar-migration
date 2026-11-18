# ICS calendar migration: 10,103 appointments from 7 calendars

When a Swiss psychotherapy practice moved to its own practice app, every therapist's appointments had to come along. They lived in seven separate private calendars. I built an importer that reads the exported ICS files and brings them into the shared calendar, so nobody had to retype an appointment.

This is a write-up. The source code belongs to the practice and is not public.

## Result

10,103 appointments from 7 calendars imported, including recurring series, exceptions and all-day entries.

## What the importer handles

- **RFC 5545 parsing:** line folding, TZID parameters, conversion to Europe/Zurich, daylight saving time.
- **Recurring appointments:** RRULE series are expanded and exceptions are respected.
- **All-day entries:** calendar apps export them differently, with and without `VALUE=DATE`. Both are recognised and shown in a separate all-day row, so the grid stays free for sessions.
- **Duplicates:** entries that already exist are skipped, so a calendar can be imported again safely.
- **Patient matching:** a short code in the appointment title links the appointment to the right patient record.
- **Preview first:** the import shows what it will create before it writes anything. People trust a migration once they can see it beforehand.

## Why a custom parser

The import had to cover exactly the cases in the practice's calendars, with a preview and patient matching on top. A focused parser for those cases was easier to test than bending a general library to fit. Recurrence expansion itself uses the rrule library.

## Stack

Node.js · Express · rrule · SQLite · React
