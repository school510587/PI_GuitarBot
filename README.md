PI_GuitarBot
============

     This is a PHP application for processing sheet music into binary serial
commands. It is a part of GuitarBot project.
     The application originally resides in PI (RaspberryPI), which sends binary
commands to STM32Discovery through serial bus. The following chapters introduce
formats of hardware configuration specification, sheet music input, and binary
serial output.

[-] Getting started
     To build and run the project, please make sure that you have installed
the command-line interface of PHP5. It can become available using the command:

     $ sudo apt-get install php5-cli

, and the command runs this project:

     $ php -e main.php <config_script> <score_script>

The two scripts will be described in detail. You can choose GuitarBot.ini as
<config_script> input.

[-] Hardware configuration <config_script>
     This script defines four latency times: move, press, fret and release. One
line is one single specification of a latency time. Their formats:

     move_latency <from_string> <to_string> <value>
     press_latency <value>
     fret_latency <value>
     release_latency <value>

     move_latency is the delay for motor movement from a position (say A) to
another position (say B). A->B and B->A can be assigned with different values,
if only one of them is assigned then they both share the same value.
     press_latency is the delay for motor movement from [release] to [press]
state.
     fret_latency is the minimum time from [fret] to [release] command that
human can hear the pitch played on a guitar.
     release_latency is the delay for motor movement from [press] to [release]
state.
     All <value> are measured in seconds, while <from_string> and <to_string>
must be positive integers.

[-] Music score <score_script>
     Music score must be a file of plaintext. Only "notes" will be fetched from
the script. One note is one or more playing commands enclosed by parentheses.
For example, (1021) is a note played with the first (the thinnest) string open
and the second string pressed at position 1. Note: The larger position number
corresponds to the higher pitch.
     There must be a duration number following each note. For example, if the
shortest note is a eighth note, then duration numbers are 1, 2, 4, and 8 for
eighth, quarter, half, and whole notes respectively. For triplets, all duration
numbers in this score should be dividable by 3, except notes of triplets. Thus,
duration numbers can always be integers, retaining its accuracy and simplicity.
     For special skill of playing a chord in guitar, there can be a ^ mark in
front of such note. Strings are fretted successively either in up-to-down or
down-to-up order, according to direction of the arrow beside the note on printed
score. To support this skill, the script requires that playing commands of such
notes should be ordered (either direction) by string numbars.
