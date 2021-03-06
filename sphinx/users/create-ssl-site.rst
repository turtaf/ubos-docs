How to create a website secured by SSL/TLS
==========================================

Creating an https website secured by SSL or TLS is notoriously difficult. UBOS makes it
easy. You have two options:

#. Self-sign your keys. This is easiest, and costs no money, but you need to set a
   security exception in your browser. (That isn't hard either, but off-putting for
   any visitor to your site that isn't you.)

#. Have an official certificate authority sign your keys. That usually takes some time
   and money, is more complicated, and requires that you own an official domain name
   for your site.

For a self-signed site, simply execute the following command::

   > sudo ubos-admin createsite --tls --selfsign

and continue to answer the questions just as you did in :doc:`firstsite`. Done!

Once the site is up, you will notice that visiting the site without the https prefix
automatically redirects you to the secure, https version.

For a site whose keys are signed by a certificate authority, you need to perform the
following steps. Let's assume you want to run ``example.org`` with SSL; replace this
with your own domain name. First, generate SSL keys::

   > openssl genrsa -out example.org.key 4096

Protect the generated file ``example.org.key``. Anybody who can get their hands on this
file can impersonate you.

Then, generate the certificate request::

   > openssl req -new -key example.org.key -out example.org.csr

This will ask you a few questions, and the generate ``example.org.csr``. Send
``example.org.csr`` to your certificate authority.

Once your certificate authority has approved your request, they typically send you
two files:

* the actual certificate. This file typically ends with ``.crt``, such as
  ``example.org.crt``.

* a file containing their certificate chain. This is the same for all of their
  customers, and might be called ``gd_bundle.crt`` (for GoDaddy, for example).

Unfortunately, different certificate authorities tend to call their files by
different names, and many are not exactly very good at explaining which is which.

Keep all of those files in a safe place. When you are ready to set up your new site,
invoke::

   > sudo ubos-admin createsite --tls

and enter the names of the files when asked. Done!

Once the site is up, you will notice that visiting the site without the https prefix
automatically redirects you to the secure, https version.
