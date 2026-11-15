# Netheret help desk, step 07: timeline

A teaching example. Over sixteen steps we build a help desk web app for
Netheret, a made-up company that supports the IT of small businesses. This
step lets staff work on a request: assign it, escalate it to a higher tier,
change its status and add updates, all recorded on a timeline.

## What this step adds

In `helpdesk/tickets.py` and `templates/tickets/detail.html`:

- **Timeline**: every `TicketUpdate` for the request, oldest first. Each one
  is marked "Internal note" or "Customer can see this".
- **Assign** (`/requests/<id>/assign`): Tier 1 agents and managers can give
  a request to anybody on the staff. Engineers get a "Take this request"
  button instead. A new request becomes open once somebody has it.
- **Escalate** (`/requests/<id>/escalate`): move a request up to Tier 2 or
  Tier 3, with a reason. It is unassigned so the next tier can pick it up.
- **Add an update** (`/requests/<id>/updates`): a note, an optional new
  status, and a tick box for whether the customer can see it.
- `add_update()` and `change_status()`: every action writes to the timeline,
  and `resolved_at` is set when a request is resolved and cleared if it is
  reopened.

## Run it

```
python -m venv .venv
source .venv/bin/activate        # on Windows: .venv\Scripts\activate
pip install -r requirements.txt
python seed.py
flask --app helpdesk run --debug
```

Sign in as `tier1@netheret.example` (demo password `password123`), open a
new request from the queue, assign it, escalate it and resolve it. Then sign
in as `tier2@netheret.example` and take a request.

## What to look at

- Each action is its own small form that POSTs to its own route, then
  redirects back to the detail page (so refreshing does not send it twice).
- The server checks the role again: an engineer who sends somebody else's id
  to `/assign` gets 403, even though the page never offers that.
- A tick box is only sent when it is ticked, so "not sent" means "no".
- Text in updates is escaped by Jinja, so `<b>` shows as text, not bold.

## Learn this step

This is step 7 of 16 of **Task 2. Developing the solution**, a free series of T Level Digital Software Development
lessons that builds the Netheret help desk in agile sprints with Python and Flask. Read the lesson for this step:
[Assigning, escalating and the timeline](https://mitsurutech.co.uk/resources/t-level/dsd-t2-07-timeline).

- Previous step: [step 06, the request queue and filters](https://github.com/mitsurutech/netheret-helpdesk-06-queue)
- Next step: [step 08, scheduling on-site jobs](https://github.com/mitsurutech/netheret-helpdesk-08-jobs)
- [Every free T Level Digital lesson](https://mitsurutech.co.uk/resources/t-level)
- [Teach these lessons to your class on MitsuruTech](https://mitsurutech.co.uk/learning), [see the pricing](https://mitsurutech.co.uk/pricing) or
  [start a free year for your college](https://mitsurutech.co.uk/signup)

## About

Written by [Mitsuru Technologies](https://mitsurutech.co.uk) (MitsuruTech) as a teaching example for T Level Digital
Software Development. The Netheret IT Support Solutions brief is from Pearson's sample assessment material; this code
and its notes are our own, and are not published, endorsed or approved by Pearson or the Department for Education.
'T Level' is a registered trade mark of the Department for Education.

