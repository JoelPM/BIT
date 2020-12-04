# BIT
Browser Interest Tooling.

A generalized form of TURTLEDOVE. The most basic element of information. Not avian.

TURTLEDOVE (abbreviated TD in places) is an elegant but high-level approach to introducing privacy in the context of ads. This document assumes that TURTLEDOVE is the right foundation and generalizes the concept while also filling in some details.

This document assumes familiarity with the [TURTLEDOVE](https://github.com/WICG/turtledove/blob/master/README.md) proposal.

# Motivation
### General Is Better: Spec APIs, not business models
The browser succeeded (in part) because it provided a strong set of tools that applied to a number of use cases beyond those for which it was originally intended. While the principles of TD are aimed at bringing privacy to browser based advertising, they can do much more. If it's possible to generalize without compromising the original intent, we should do so.

### Name It Right (counter example: XMLHTTPRequest)
XMLHTTPRequest was an example of a specifically named tool that found broad adoption outside of XML. Advertising is just content. The goal of TD is to enable *interest based content customization in a privacy preserving manner*. Let's name it appropriately.

### 

# Interest Groups (IG)
BIT broadens the definition of an interest group:
> An "interest group" is a collection of people whom ~~an advertiser~~ a content producer believes will be interested in seeing some ~~type of ad~~ form of content.

The definition in the TD spec holds. The example is slightly modified:

```
const myGroup = {'owner': 'guitarworld.com',
                 'name':  'telecaster-player',
                 'readers': ['premierguitar.com',
                             'reverb.com']
                };
navigator.joinAdInterestGroup(myGroup, 30 * kSecsPerDay);
```

Th

BIT also makes explicit some requirements for IG privacy and handling:
* Interest Group membership is only persisted in the user's browser in the IG Registry* (not cookies, not local storage, not ...)
* Interest Groups do not contain any information beyond a name, an owner, and readers. They are binary signals indicating that a user has an interest.
* Interest Groups are only ever exposed outside the browser in the context of Interest Targeted Content Requests or Interest Targeted Content Reporting.

Beyond this, the original TD requirements for Interest Groups hold.

# Interest Targeted Content (ITC)
ITC is content packaged for display to a member of an interest group. It may be an advertisement or it may be an article, it doesn't matter. What does matter is how the content is packaged. As in TD, ITC is expected to be packaged as a [Web Bundle](https://web.dev/web-bundles/).

## Interest Targeted Content Request
As in TD, ITC Requests are sent by the browser using a solution that:
1) Prevents timing attacks
2) Optimizes the availability of Interest Targeted Content
3) Efficiently utilizes the network

The implementation will likely vary based on device type and network bandwidth.

In the interest of efficiency, ITC Requests can contain multiple IGs when the combination of IGs does not pose a danger of uniquely identifying a user.

If an IG owner wishes to receive ITC Requests it should be listed as a Reader.

An ITC Request is a standard GET request (as in TD):
```
GET https://reverb.com/.well-known/itc?interest_group=guitarworld.com_telecaster-player,guitarworld.com_fender-fan
```

The response to an ITC Request is JSON that describes where the content can be found, the interest groups it corresponds to, meta-data governing the eligibility of the content for display, and other things I haven't thought of yet:

```
[{'group-owner': 'guitarworld.com',
  'group-name': 'telecaster-player',
  'itc-cbor-url': 'https://reverb.com/itc/ads/ad-123456789.wbn',
  'cache-lifetime-secs': 3600,
  'min-period-seconds': 120,
  'max-display-count': 10,
  'selection-logic-url': 'https://reverb.com/itc/selection.js',
  'selection-signals': {
    'type': 'ad',
    'atf_value': 250,
    'btf_value': 25,
    'brand': 'fender',
    'exclusions': 'gibson'
  }
}]
  
```

## Interest Targeted Content Selection
When a browser has an opportunity to render ITC it may need to be Multiple pieces of ITC may be available when 

## Interest Targeted Content Rendering

## Interest Targeted Content Reporting
