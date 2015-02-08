# Reachability.swift

Replacement for Apple's Reachability re-written in Swift with callbacks

Inspired by https://github.com/tonymillion/Reachability 

**NOTES:**

- As of Swift 1.1, you cannot convert Swift closures into C-function pointers, meaning we can't set an `SCNetworkReachabilityCallBack`. To get around this, this reachability replacement uses an `NSTimer` firing at 1/2 second intervals.

- If an application has the privacy option “Use cellular data” turned off, the Reachability class still reports isReachable() to be true. There is currently no (non-private) API to detect this. If you need this feature, please raise file a [bug report](https://bugreport.apple.com) with Apple to get this fixed. See devforums thread for details: https://devforums.apple.com/message/1059332#1059332

## Use

Just drop the **Reachability.swift** file into your project. That's it!

## Example - closures

    let reachability = Reachability.reachabilityForInternetConnection()

    reachability.whenReachable = { reachability in
        if reachability.isReachableViaWiFi() {
            println("Reachable via WiFi")
        } else {
            println("Reachable via Cellular")
        }
    }
    reachability.whenUnreachable = { reachability in
        println("Not reachable")
    }

    reachability.startNotifier()

## Example - notifications

    let reachability = Reachability.reachabilityForInternetConnection()

    NSNotificationCenter.defaultCenter().addObserver(self, selector: "reachabilityChanged:", name: ReachabilityChangedNotification, object: reachability)
    
    reachability.startNotifier()

and

    func reachabilityChanged(note: NSNotification) {

        let reachability = note.object as Reachability

        if reachability.isReachable() {
            if reachability.isReachableViaWiFi() {
                println("Reachable via WiFi")
            } else {
                println("Reachable via Cellular")
            }
        } else {
            println("Not reachable")
        }
    }


## Let me know!

If you use **Reachability.swift**, please let me know… and tell your friends! 

Cheers,
Ash

