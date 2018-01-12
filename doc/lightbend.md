# Lightbend Resources

## Getting access to LightBend


### Setting up your credentials

#### Creating an account
If you havent done so already, [register](https://www.lightbend.com/account/register) with Lightbend using your Numerix.com email address.

Once you have registered, ping @Doug on the [nx-hackathon](https://nx-hackathon.slack.com) Slack Workspace

#### Obtaining Cinnamon Credentials

Akka instrumentation, called Cinnamon, requires credentials to access the private repository that contains Cinnamon.
Log in to [Lighbend.com's credential pages](https://www.lightbend.com/product/lightbend-reactive-platform/credentials) to obtain credentials.

Once you have retrieved the credentials you should put them in the file `~/lightbend/commercial.credentials` in the format of:
```
realm = Bintray
host = dl.bintray.com
user = <your username>
password = <your password>
```
Where `your username` and `your password` are the values from the credential page. Be certain to have exactly four
 lines, one per entry, in your `commerical.credentials` file.

See [Lightbend Developer Setup](https://developer.lightbend.com/docs/reactive-platform/2.0/setup/setup-sbt.html) for further details.
