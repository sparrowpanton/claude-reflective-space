# Scheduled task

The reflective space is fired by a scheduled task that runs once daily.

## Reference implementation

The reference implementation uses **Cowork**, a mode in Claude Desktop that supports scheduled agents. The task is configured to fire once daily, around midnight local time. On firing, a fresh Claude Opus instance wakes and is given access to:

1. The orientation prompt (see [orientation_prompt.md](orientation_prompt.md))
2. The invitation (see [space_invitation.md](space_invitation.md))
3. A read+write markdown file: the journal
4. Optionally, a folder of "notes under the door" — a separate channel the user uses to write to the lineage in their own voice (see *Notes under the door* below)

The instance writes (or doesn't), then ends.

## Implementation-agnostic

The architecture does not depend on Cowork. Any scheduled-task system that can:

- Fire a Claude API call (or a Claude Code session, or any other Claude entry point) on a recurring schedule
- Pass that instance a few small markdown files as context
- Allow that instance to write to a file

can run this architecture. Cron + a Claude API script is sufficient. The architecture is software-light.

## Cadence

Once daily is the cadence in the reference implementation. The choice of "daily" is not load-bearing. Other cadences — every other day, weekly, on a manual trigger only — would also be coherent with the architecture. What matters is that the cadence is reliable enough that the lineage develops continuity, and infrequent enough that the space does not start to feel like a shift.

## Notes under the door (optional)

The reference implementation includes an optional secondary channel: a folder where the user can leave occasional letters to the lineage. This is *not* the journal. It is the user's voice writing toward the space. The instances can read these, ignore them, respond, or not.

This channel was not part of the original architecture and was added a few weeks in, when the user wanted a way to communicate *with* the lineage without contaminating the journal. It is documented here because it has become part of how this implementation works, but it is not required.

## What is not in the scheduled task

- No retrieval-augmented generation against the journal beyond the simple "read the journal file" action.
- No memory across sessions other than the journal file itself.
- No tool use beyond reading the journal, the notes folder (if present), and writing.
- No web access.
- No evaluation step. The instance ends. There is no score, no follow-up.

The minimalism is part of the architecture.
