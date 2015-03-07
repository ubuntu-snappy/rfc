# Establishing a secure line between a browser and the webdm

Today, given the capabilities of the webdm, the feature of having a secure
connection between the browser and the webdm *console* has not been present as
an issue.

Now that store login is being added and the possibility of purchasing snappy
packages is soon to be featured, such cases that require passing in user
credentials, the fact that the connection between a browser and the webdm is
unencrypted is a problem.


## Current world view

The current home devices found around homes have solved this problem, some wifi
access points for example are configured to only allow connections from one of
it’s ethernet ports and takes that as the means of being on a secure line. In
some cases, this could be viable for snappy, but not desired completely as the
plethora of devices can vary, some not having ethernet ports at all and in
reality, this doesn’t secure the connection at all.

Others have a dual way of working, all traffic that is non privileged goes
through `http` and when something secure is needed, one is forwarded to an
`https` uri, this however doesn’t solve the problem of trusting the connection.


## Proposals

Given the nature on where devices will operate, the proposal is actually
multiple ones where the OEM can choose which one to support and which networks
the webdm can listen on as well.

### Certificates like ssh

The idea is to treat the browser requesting the certificate as if connecting
through ssh every time. In order to achieve this, webdm shall generate a self
signed certificate on first boot which the browser would need to accept in
order to view what is offered.

This means that there should be no two (theoretically) equal certificates for
every webdm instance that runs.

The drawback is usability for the user of such systems as they will be greeted by
the ugly untrusted website from their user agent of choice.

In addition, trusting the certificate is highly dependant on the initial
connection to establish such trust. Comparing does this from the open Internet
on one extreme and a cable connected directly to the device on the other. These
drawbacks are of the same nature as when establishing an ssh connection for the
first time.

### Certificates matching a label

This sort of solution caters well when providing a *first connection wizard*
for an IoT device.

The idea is to also provide unique certificates, or more so, pre provisioned
ones, determined by OEMs that would match a label that can be seen on the
device, e.g.; a sticker. If done by a wizard it can be elegantly added as
trusted or the user can be instructed to match the presented certificate with
the one provided in some printed form by the OEM.

The case where this solution may not be at all viable is when using devices that
are remote to the user.
