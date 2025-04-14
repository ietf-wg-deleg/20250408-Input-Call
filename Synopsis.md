## Adoption Preferences - Mailing List

| draft-homburg | draft-wesplaap | No preference stated |
|---------------|----------------|----------------------|
| ~10 | ~22 | 5 |

## Feedback, by requirement

| Requirement | draft-homburg | draft-wesplaap |
|-----------|---------------|----------------|
|H1|ok|ok|
|H2|ok|Recursive resolvers without support for DELEG can respond to DELEG queries with two completely different answers depending on the state of the cache<br>-  the contents of a zone should not be used to signal the properties (DNSKEY bit) of the software running on nameservers that serve the zone<br>- No obvious path forward to a mechanism for priming queries. The DELEG RRtype is parent-side and cannot be used for this purpose. SVCB records cannot be placed at the apex of a zone and this draft renamed some parameters making it incompatible with SVCB|
|H3|it is not clear to me that non-deleg resolvers can safely handle unsolicited _deleg records in a referral response<br><br>Additional load to all nameservers until they are upgraded<br><br>the burden of cost of deployment is unfairly balanced. Authoritative servers need to upgrade regardless of deploying deleg to avoid additional load from deleg aware validating resolvers|ok|
|H4|ok|ok|
|H5|ok|ok|
|H6|ok|ok|
|H7|ok|ok|
|S1|ok|ok|
|S2|ok|ok|
|S3|it comes with significant additional query load at delegation centric zones<br><br>Improvement gains in deployability comes at the cost of increased resource consumption, notably on the wire<br><br>Generates more traffic than DELEG which, for most zones will be amortised over many queries. Performance may be significantly poorer for deep and sparsely populated zones like ip6.arpa|ok|
|S4|ok|ok|
|S5|ok|ok|
|S6|The additional branch (_deleg) method is not scaleable to future protocol extensions (say _deleg2 or _deleg3)|ok|
|S7|ok|ok|
|Other|short term wins but more long term tech debt<br><br>(S4?) More confusing to explain to others<br><br>The additional complexity of the high level design is not worth the short term gains<br><br>Harder to implement cleanly into the existing code of BIND 9<br><br>Implications of bogus or malicious use of a _deleg label in a domain name and how that affects resolution have not been considered|the DE bit required<br><br>The special handling required for a new parent-side record at the zone cut<br><br>(H6, S4?) requiring authoritative servers, resolvers, and forwarders to have new code before it will work<br><br> It should set aside a range of additional parent side records in the future, so we can avoid using another DNSKEY flag and EDNS0 flag in the future (this work should be dependent on specifications, such as the reservation of parent-side record types, like draft-peetterr-dnsop-parent-side-auth-types-01)<br><br>The need for some extra DNSSEC work|

## Feedback, verbose
   * General
       - The drafts should show a desired end-goal.  Assuming either a clean slate or the assumption that all needed elements are upgraded from today’s code bases, what would be in a DELEG resource record?  How would a parent delegate a subdomain to multiple DNS-hosters?  Without some sense of a “complete solution” it’s hard to ever know if all corner cases are identified, hard to see how the end state is an improvement on the current state.
       - Both drafts are trying to solve the wrong problem
   * draft-homburg
      - it comes with significant additional query load at delegation centric zones, violating soft-requirement S3.
      - the burden of cost of deployment is unfairly balanced. Authoritative servers need to upgrade regardless of deploying deleg to avoid additional load from deleg aware validating resolvers, violating H3.
      - it is not clear to me that non-deleg resolvers can safely handle unsolicited _deleg records in a referral response
      - The additional branch (_deleg) method is not scaleable to future protocol extensions (say _deleg2 or _deleg3)
      - short term wins but more long term tech debt
      - Additional load to all nameservers until they are upgraded
      - More confusing to explain to others
      - The additional complexity of the high level design is not worth the short term gains
      - Harder to implement cleanly into the existing code of BIND 9
      - Improvement gains in deployability comes at the cost of increased resource consumption, notably on the wire
      - Introduces a special, magic label in an owner-name to change resolver behaviour
      -  Implications of bogus or malicious use of a _deleg label in a domain name and how that affects resolution have not been considered
      -  Generates more traffic than DELEG which, for most zones will be amortised over many queries. Performance may be significantly poorer for deep and sparsely populated zones like ip6.arpa
    
   * draft-wesplaap
       - The special handling required for a new parent-side record at the zone cut
       - the DE bit required
       - requiring authoritative servers, resolvers, and forwarders to have new code before it will work
       - It should set aside a range of additional parent side records in the future, so we can avoid using another DNSKEY flag and EDNS0 flag in the future
       -  this work should be dependent on specifications, such as the reservation of parent-side record types, like draft-peetterr-dnsop-parent-side-auth-types-01
       - Recursive resolvers without support for DELEG can respond to DELEG queries with two completely different answers depending on the state of the cache
       -  the contents of a zone should not be used to signal the properties (DNSKEY bit) of the software running on nameservers that serve the zone
       -  No obvious path forward to a mechanism for priming queries. The DELEG RRtype is parent-side and cannot be used for this purpose. SVCB records cannot be placed at the apex of a zone and this draft renamed some parameters making it incompatible with SVCB
       -  The need for some extra DNSSEC work
