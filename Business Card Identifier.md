# Business Card Identifiers

One of the technology decisions faced by the incomming Clinton administration was whether to continue 
deployment of the OSI based DECNET PhaseV email infrastructure purchased during the Bush administration
or switch to Internet email. Ira Magaziner wrote a series of pithy comments on the topic, but there was 
one advantage of Internet email addresses that stood out above all others: *An Internet email address 
can be written on a business card*.

The quote is from personal recollection, but there are actually two separate issues here:

1) Can the address be printed on the card and typed into a computer?
2) Can someone write their identifier on a business card sized scrap of paper.

If we accept use of QR codes, the first is not really much of a constraint. Any reasonably compact 
machine readable identifier can be expressed in URI form and plonked onto a business card. But
that form of identifier can't be read over the phone, printed on a slide deck or paper etc.

The second test provides a much better proxy for the question of identifier usability: How easy is 
it for humans to pass their identifier on to other humans during face to face human interactions that
are not mediated by a machine?

In this note, I first examine various criteria that may be used to judge suitability for a business-card
friendly identifier format, then propose a format for an identifier and provide a brief sketch of how such
a scheme might be implemented.

## Comprehensibility

Another Internet related identifier that has fallen out of favor after a brief period of use is the 
PGP key fingerprint. In practice, a PGP fingerprint on a business card typically transmites exactly
one bit of information: The card holder accepted PGP email at the time the card was printed.

Since a PGP fingerprint consits of a random sequence of hexadecimal digits, reading one out
over a telephone is a chore as is writing one down from a presentation. I cannot remember any of the
PGP fingerprints I have used over the years and I have precisely no inclination to try.

A good rule of thumb is probably to ask how many words it takes to read the identifier over
a telephone. A 128 bit hexadecimal fingerprint is 32 words, 35 if group separators are used. 
While [Mesh/UDF](https://www.ietf.org/archive/id/draft-hallambaker-mesh-udf-15.html) 
fingerprints use BASE32 encoding for a slightly more compact encoding, the result is still
not 'business card compatible':

MB5S-R4AJ-3FBT-7NHO-T26Z-2E6Y-WFH4

**Conclusion:** Fingerprint based identifiers are only suitable for use as machine readable identifiers
and very limited human interactions. It may under certain circumstances be acceptable to require
a system administrator to enter a 28 character identifier. It might be acceptable to require a 
user to compare two identifiers and verify that they are the same. It might be acceptable to 
require a user to cut and paste a PIN value that is a fingerprint.

## Eliminating Clutter

News organizations refer to intrusions on the viewers time as 'clutter'. Clutter includes 
advertisements, previews, trailers and the chyrons running along the bottom of the screen.

Rather than ask _how much clutter will the user bear?_, we should instead flip the question 
around and ask _what is the absolute minimum of clutter?_.

The absolute minimum of clutter is of course no clutter at all. So if Alice is lucky enough to
obtain the identifier 'alice', her business card might be:

~~~~
Alice Toklas
tel: +1 666-555-1234
email: alice@example.com
twitter: @alicetwitter
bluesky: alice
~~~~

Where 'bluesky' is a placeholder for whatever name the scheme is launched to the public under.

\[We note in passing that Alice no longer provides a fax or postal address on her card.]

While this identifier works in the narrow confines of the business card, it only works in a 
conversation if the user calls it out as an identifier. This seems to suggest that people do
require an indicator that a word is an identifier. Following Tomlinson, the @ character is 
traditional.

We thus have a choice of @alice or alice@. The form @alice is preferred, not least because 
it provides an editor with an advance indication that an identifier is being entered, allowing
user assistance.

~~~~
Alice Toklas
tel: +1 666-555-1234
email: alice@example.com
twitter: @alicetwitter
bluesky: @alice
~~~~

# Post Identity Singularity

The Identity Singularity is a hypothetical future in which an identifier has achieved 
widespread use such that there is no reason to use any others. Achieving such a position
does of course require that the identifier have certain properties, including:

1. Be managed as public goods
2. Allow incorporation of other identifier spaces
3. Provide binding to authentication scheme presenting high work factor
4. Be available to any party

In this hypothetical future, Alice's business card is much smaller:

~~~~
Alice Toklas
@alice
~~~~

There is no need for Alice to specify any other identifier because her @alice provides an
authenticed, authorized party with access to all the other interaction modalities.

One consequence of such a singularity is that all of Alice's identities have collapsed
into a single identifier. Thus if Alice wishes to maintain separate identities, she must
obtain multiple identifiers. Since she has been told 'golf is not very popular around her',
Alice has a separate identity for everything related to golf:

~~~~
Alice Toklas
@alt_alice
~~~~

Alice also has a separate identity for managing the IoT devices in her home. Her appartment
is rented and the devices themselves belong to the landlord but they are under Alice's 
control for the duration of the rental. This state of affairs is most easily managed by 
attaching the devices in the appartment to a separate identity @alice_flat which can be
transfered to the new tennant if Alice moves.

## Delegation

Alice is a person, a message directed to @alice is clearly intended to be read by one of the 
devices Alice has assigned the task of receiving messages to her person. But how do does
Alice's thermostat address a message to her furnace? If Alice has multiple residences, hos
does she direct a message to a specific device at one of them?

The form \<subordinate>@\<name> was established for this purpose in RFC822. The identifier
@alice is a first class name because it is a name that is a signifier that is assigned 
directly to the signified itself. The identifier hvac@alice is a second class identifier
because the interpretation of 'hvac' only has meaning with respect to the first class name
@alice.

## Incorporation
  
Incorporation is a specific form of delegation in which a second class identifier in one name
space refers to a first class identifier in another.

For example, Alice might wish to direct Bob to her Twitter page. If she has a single Twitter 
handle, she can do this by giving her callsign @alice. But if she has multiple handles,
she needs to be able to specify the particular one.

We assume that the callsign @twitter is held by Twitter. The identifier alicetwitter@twitter
then provides a clutter-free means of specifying Alice's twitter handle @alicetwitter.

The interpretation of such callsigns is of course a matter for the holder of the first class 
identifier to determine. This might be performed by means of a signed assertion containing
some form of regular expression or substitution expression. A JSON mapping assertion specifies
mappings for Alice's Twitter web feed and also a means of contacting her directly through a
VOIP service that has been contracted out to provide person to person communications using
Twitter handles and access control lists (i.e. blocklists):

~~~~
"Mapping" : {
    "web" : "http://twitter.com/$id",
    "voice" : "yavp://voicecorp.com/$id"
}
~~~~

\[The details of such a format can be left to future discussion.\

So from the user experience point of view, if Bob is in the Twitter app and is responding to Alice, 
he can make use of a (hypothetical) in-app VOIP feature to contact Alice by her handle
@alicetwitter. 

If however, Bob is starting a call from his multi-protocol VOIP application, he selects
the entry for @alice. This contains a signed contact assertion which might be public or might
be private and specific to just Bob. This contains a list of preferences:

~~~~
"voice" : [
    "voice:alice1@signal", "voice:alicetwitter@twitter", "tel:+1-666-555-1234"
    ]
~~~~

\[Again, we can endlessly bikeshed the identifier presentation, URIs, whatever. ]

Alice and her various providers both have the ability to control the mapping of the
global name to the various communications modalities people might wish to use to initiate
a call with her.

Alice has the ability to tell Bob 'I prefer you call me on Signal' while telling everyone 
else they should reach her on Twitter or plain old telephone service.

## Protocol Transition

The Web as originally presented at the Annecy conference in 1992 was not a single protocol, 
it was a framework through which multiple existing protocols plus HTTP could be reached. The
early Web libraries supported Gopher, WAIS, Z39, FTP and NNTP.

This state of affairs did not last very long of course, within a very short period, every one
of the original protocols had been effectively abandoned for HTTP.

Today, the telephone system and SMTP email are both slowly dying due to the lack of effective
security controls to prevent abuse. Any artifact which allows any person to intrude on the time
of anyone else on the planet at any time of the day or night at negligible cost is going
to be replaced very quickly when a viable alternative becomes available.

Neither Skype, nor Signal nor any other walled garden system can replace the telephone system for
the singular reason that they are a walled garden. The telephone system is ubiquitous because
it is the only ubiquitous service every person that might be contacted supports.

A global identifier that offers sufficient guarantees with respect to openness and security 
to be widely accepted, provides a natural path enabling the replacement of the no longer fit
for purpose legacy infrastructures of telephone and SMTP with a compact set of modern alternatives.

A future in which it is not possible to introduce new communications modalities or a particular
modality is limited to a single protocol is clearly undesirable. But the current situation in
which the legacy incumbent is challenged by hundreds of walled garden alternatives represents
a stalemate.

The architecture outlined in this note describes a system which encourages the market to consolidate 
protocol choices wherever possible while also supporting and encouraging deployment of new
protocols that offer sufficient advantages to be worthwhile.
