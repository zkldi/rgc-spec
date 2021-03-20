.. _std/iidx-SP:

std/iidx-SP
==================================

################
Format Context
################

****************
Associated Game
****************

`beatmania IIDX <https://en.wikipedia.org/wiki/Beatmania_IIDX>`_

********************
Associated Filetypes
********************

- ``.1``

***********
Description
***********

This is the std library format for beatmania IIDX's Single Player (SP) game mode. In game, this mode utilises 7 keys and a turntable.

************************
Latest ``formatVersion``
************************

1

####################
``meta`` Extensions
####################

.. module:: meta

.. data:: genre

    The genre for the song. If not known, this should be null.

    :type: String | null

.. data:: difficulty

    The difficulty for this chart. If not known, this should be null.

    .. note::
        Formats such as `.1` have no concept of ``null`` difficulties, passing a null difficulty to a serialiser will likely result in a warning.

    .. warning::
        This difficulty list does not include LIGHT7, 7KEYS or BLACK ANOTHER.
        The reason for this is that all of these have a modern counterpart.

    :type: "BEGINNER" | "NORMAL" | "HYPER" | "ANOTHER" | "LEGGENDARIA" | null


.. data:: level

    The difficulty level for this chart. If not known, this should be null.

    .. note::
        7+ and 8+ are legacy levels, and have not been used since IIDX12 in 2005.

    :type: "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9" | "10" | "11" | "12" | "7+" | "8+" | null

.. data:: marquee

    The text that appears on the LED Ticker during play. If not known, or not present, this should be null.

    :type: String | null

######################
``events`` Extensions
######################

.. module:: events

.. warning::
    As stated in overview, all events **MUST** inherit :ref:`rgc_event`.
    As such, all events defined below **ALSO** have the properties of :ref:`rgc_event`.

.. data:: NOTE

    The notes of the chart. These are played by the 1-7 keys on the controller.

    .. attribute:: NoteEvent.col

        The column this note is for, starts from 0.

        :type: 0 | 1 | 2 | 3 | 4 | 5 | 6

    :type: List<NoteEvent>

.. data:: SCRATCH

    The scratch notes in the chart. These are played by the turntable.

    :type: List<:ref:`rgc_event`>

.. data:: CN

    The Charge Notes (CNs) in the chart.

    These are held notes, with release timing windows.

    .. attribute:: CNEvent.col

        The column this CN is on, starts from 0.

        :type: 0 | 1 | 2 | 3 | 4 | 5 | 6

    .. attribute:: CNEvent.msEnd

        The exact time in miliseconds this CN ends at.

        :type: Positive Float

    :type: List<CNEvent>

.. data:: HCN

    The Hell Charge Notes (HCNs) in the chart.

    Identical to CNs, but holding these will give the user gauge continuously, and dropping them will lose it continuously.

    The properties for this event are identical to the properties for ``CNEvent``.

    .. note::
        As of writing this, charts in-game may ONLY have either CNs OR HCNs.
        However, this may not be the case forever, and as such, these are independent.

    :type: List<CNEvent>

.. data:: BSS

    The Backspin Scratches (BSSes) in the chart.

    These are essentially hold-scratches, where the user would have to keep spinning the turntable, then spin the other way at the end.

    .. attribute:: BSSEvent.msEnd

        The exact time in miliseconds this CN ends at.

        :type: Positive Float

    :type: List<BSSEvent>

.. data:: HBSS

    Identical to BSSes, but 'holding' these will give the user gauge continuously, and dropping them will lose it continuously.

    The properties for this event are identical to the properties for ``BSSEvent``.

    .. note::
        Like HCNs and CNs: charts in-game may ONLY have either BSSes OR HBSSes.
        However, this may not be the case forever, and as such, these are independent.

    :type: List<BSSEvent>

.. data:: BPM

    Indicates a BPM change in the chart.

    .. attribute:: BPMEvent.bpm

        The BPM the chart has changed to.

        :type: Positive Non-zero Float

    :type: List<BPMEvent>

.. data:: METER

    Indicates a Timing Meter change in the chart. (Such as 4/4 => 7/4).

    .. note::
        This is rarely used in game, even in scenarios where it should be used.

    .. attribute:: MeterEvent.num

        The numerator for the meter change.

        :type: Positive Non-zero Integer

    .. attribute:: MeterEvent.denom

        The denominator for the meter change.

        :type: Positive Non-zero Integer

    :type: List<MeterEvent>

.. data:: SAMPLE

    Attaches an audio sample to a key.

    .. attribute:: SampleEvent.col
        
        The column this audio sample is attached to.

        .. warning::
            Despite SP only having 7 playable columns, it is perfectly legal (and used in real charts) to place samples on
            any of the 14 keys.

        :type: Bounded Integer [0,13]

    .. attribute:: SampleEvent.sound
        
        The sound this is meant to play. It is not currently documented how this works, but it presumably reads from the associated ``.2dx`` archive.

        :type: Positive Integer

    :type: List<SampleEvent>

.. data:: BGM

    Indicates that there is an audio file meant to be played here.

    .. attribute:: BGMEvent.pan
        
        Where to pan this audio from. 0 is left-most, 16 is right-most, and 8 is the center.

        :type: Bounded Integer [0,16]

    .. attribute:: BGMEvent.sound
        
        The sound this is meant to play. Like ``SampleEvent``, It is not currently documented how this works, but it presumably reads from the associated ``.2dx`` archive.

        :type: Positive Integer

    :type: List<BGMEvent>

.. data:: MEASUREBAR

    Indicates that there is a measure bar meant to be displayed here.

    .. note::
        Interestingly, this is a specific event in the game, and `can be omitted completely from the chart. <https://www.youtube.com/watch?v=1joKw32ahG4>`_

    :type: List<:ref:`rgc_event`>

.. data:: EOC

    Indicates that this is the end of the chart. This is presumably used in game to instruct the game to fadeout into displayscore.

    .. note::
        It appears to be legal to have multiple of these in one chart, but the first one will trigger the fadeout.

    :type: List<:ref:`rgc_event`>

.. data:: TIMINGWINDOW

    Changes the timing windows of the chart.

    .. warning::
        Without defining these, the game will default to no timing windows for anything other than PGREATs, which is near-unplayable.
        That is to say, these are **NOT** implicitly defined.

    .. attribute:: TimingWindowEvent.window

        The judgement this instruction is for.

        :type: "lateBad" | "lateGood" | "lateGreat" | "earlyGreat" | "earlyGood" | "earlyBad"

    .. attribute:: TimingWindowEvent.val

        The timing window (in frames) that should be assigned to this judgement.

        .. note::
            The default timing windows are:
            
            .. list-table::
                
                *   - lateBad
                    - -16
                *   - lateGood
                    - -6
                *   - lateGreat
                    - -1
                *   - earlyGreat
                    - 3
                *   - earlyGood
                    - 8
                *   - earlyBad
                    - 18
            
            Perfect Greats are implicitly assigned frames 0 and 1 by the game engine.

        :type: Bounded Integer [-127, 128]

    :type: List<TimingWindowEvent>