## NAME
    Mojo::AMI - Pure-Perl non-blocking I/O Asterisk Manager Interface, Mojo version
 
### VERSION
    0.2

### DESCRIPTION
    This module provides a dependable, event-based interface to the asterisk manager interface. http://www.voip-info.org/wiki/view/Asterisk+manager+API
    This is done with Mojo::IoLoop

### SYNOPSIS
  #### Non-blocking
    ```perl
    my $ast = AMI->new(user => 'admin', secret => 'redhat');
    
    my $loop = Mojo::IOLoop->delay(
        sub {
          my $delay = shift;
          $ast->execute({ Action => 'Login', Username => $ast->user, Secret => $ast->secret }, 
          $delay->begin)->execute({ Action => 'QueueStatus', 'Async' => '1'}, $delay->begin);
        },
        sub {
            my ($delay, $err, $response, $err2, $response2) = @_;
            print Dumper $response2;
        }
    )->wait;
    ```