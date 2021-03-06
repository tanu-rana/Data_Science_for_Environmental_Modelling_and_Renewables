    library(ismev)

    ## Loading required package: mgcv

    ## Loading required package: nlme

    ## This is mgcv 1.8-23. For overview type 'help("mgcv-package")'.

    library(quantreg)

    ## Loading required package: SparseM

    ## 
    ## Attaching package: 'SparseM'

    ## The following object is masked from 'package:base':
    ## 
    ##     backsolve

    data(wavesurge)
    par(mar=c(5.1,4.1,2.1,2.1))
    plot(wavesurge, xlab="Wave height [m]", ylab="Surge [m]")
    taus <-  seq(0.05,0.95, by=0.05)
    qrm <- rq(wavesurge$surge~wavesurge$wave,tau=taus)
    beta <- coef(qrm)
    select <- c(2,5,10,15,18)
    cols <- c(4,3,2,3,4)
    for (i in seq_along(select)) {
        beta <- coef(qrm)[,select[i]]
        abline(beta, col=cols[i], lwd=2)
        corners = par("usr") 
        par(xpd = TRUE) 
        text(x = corners[2]+.1, y = beta[1]+beta[2]*corners[2] , paste0(100*taus[select[i]],"%"), srt = 0, adj=c(0,0.5), col=cols[i])
        par(xpd=FALSE)
    }

<img src="quantreg_files/figure-markdown_strict/unnamed-chunk-1-1.png" width=".45\textwidth" />

    library(ismev)
    library(quantreg)
    data(wavesurge)
    par(mar=c(5.1,4.1,2.1,2.1))
    plot(wavesurge, xlab="Wave height [m]", ylab="Surge [m]", type="n")
    beta <- coef(rq(wavesurge$surge~wavesurge$wave,tau=0.9))
    polygon(c(7,7,8,8),c(par()$usr[4], beta[1]+beta[2]*c(7,8), par()$usr[4]),col=grey(0.8), border=NA, density=45)
    polygon(c(7,7,8,8),c(par()$usr[3], beta[1]+beta[2]*c(7,8), par()$usr[3]),col=grey(0.8), border=NA, density=45, angle=305)
    text(7.5,par()$usr[3], "90%", cex=1.5, col=grey(0.7), adj=c(0.5,-0.1))
    text(7.5,par()$usr[4], "10%", cex=1.5, col=grey(0.7), adj=c(0.5,1.1))
    points(wavesurge)
    taus <-  seq(0.05,0.95, by=0.05)
    qrm <- rq(surge~wave, data=wavesurge, tau=taus)
    beta <- coef(qrm)
    select <- c(2,5,10,15,18)
    cols <- c(4,3,2,3,4)
    for (i in seq_along(select)) {
        beta <- coef(qrm)[,select[i]]
        abline(beta, col=cols[i], lwd=2)
        corners = par("usr") 
        par(xpd = TRUE) 
        text(x = corners[2]+.1, y = beta[1]+beta[2]*corners[2] , paste0(100*taus[select[i]],"%"), srt = 0, adj=c(0,0.5), col=cols[i])
        par(xpd=FALSE)
    }

<img src="quantreg_files/figure-markdown_strict/unnamed-chunk-2-1.png" width=".45\textwidth" />

    plot(summary(qrm), parm="(Intercept)", ols=FALSE)
    points(taus[select], coef(qrm)[1,select], col=cols, pch=16)
    abline(v=taus[select], col=cols, lty=3)

<img src="quantreg_files/figure-markdown_strict/unnamed-chunk-3-1.png" width=".45\textwidth" />

    plot(summary(qrm), parm="wave", ols=FALSE)
    points(taus[select], coef(qrm)[2,select], col=cols, pch=16)
    abline(v=taus[select], col=cols, lty=3)

<img src="quantreg_files/figure-markdown_strict/unnamed-chunk-4-1.png" width=".45\textwidth" />
