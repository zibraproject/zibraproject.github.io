---
layout: post
title:  "Designing a multiplex PCR scheme for Zika"
categories: protocols
author: josh
---

#Protocol for tiling amplicon generation for MinION sequencing

##Description

This protocol for tiling RT-PCR with random hexamer priming for reverse transcription and multiplex gene specific primers for PCR. You will use two-step reagents (Protoscript II for RT and OneTaq for PCR) instead of the one-step RT-PCR you have been using until now.

## Reverse transcription

- Mix the following in a PCR tube

		2 ul Template sample 58
		2 ul Random hexamers (50 ng/µl)
		4 ul Nuclease-free H2O

- Mix by inversion

- Heat in the thermocycler at 65°C for 5 minutes then place on ice

- Add the following

		10 ul ProtoScript II Reaction Mix (2X)
		2 ul ProtoScript II Enzyme Mix (10X)

- Place on the thermocycler and start the following program

		25°C for 5 minutes
		48°C for 15 minutes
		80°C for 5 minutes

## PCR
 
- Make a master mix of the following:

	10 ul x 4.5 = 45 ul 5X OneTaq Standard Reaction Buffer
	1 ul x 4.5 = 4.5 ul 10 mM dNTPs
	0.25 ul x 4.5 = 1.13 ul OneTaq DNA Polymerase
	2 ul x 4.5 = 9 ul cDNA reaction mixture
	31.75 ul x 4.5 = 142.9 ul H2O

- Mix by inversion

- Pipette 45 ul into PCR tubes 1-4

- Add the primers

		2.5 ul F control primer (10uM) or primer pool
		2.5 ul R control primer (10uM) or primer pool

- PCR program:

		Step 1
		94°C 30 seconds 
		Step 2 (35 cycles)
		95°C 15 seconds
		59°C 5 minutes
 		68°C 30 seconds
		Step 3
		68°C 5 minutes

## Primers

Primers starting with the name ZKV_ are Charyl’s primers and need diluting from 100 uM to 10 uM before using.

For multiplex reactions mix 1 ul of all forward and reverse primers, use 5 ul for a 50 ul reaction.

###Control reactions

	1.	400_7F and 400_7R
	2.	400_14F and 400_14R

###Multiplex reaction

####Pool 1

	ZKV_01F	200_4_R_2
	200_6_F_1	200_10_R_2
	200_16_F_1	200_20_R_2
	200_26_F_3	200_28_R_2
	200_33_F_1	200_36_R_2
	ZKV_07F	200_43_R_2
	200_47_F_3	200_51_R_2
	200_55_F_1	200_59_R_2
	200_65_F_1	200_70_R_2
	400_21F_0	200_80_R_2
	200_85_F_1	200_89_R_2
	200_93_F_1	200_97_R_4
	200_102_F_1	200_106_R_2
	200_111_F_1	200_114_R_2
	200_123_F_3	200_125_R_2
	200_131_F_1	200_136_R_2
		
####Pool 2
	200_2_F_1	200_5_R_2
	200_11_F_1	200_15_R_2
	200_21_F_1	200_25_R_2
	400_8F_0	200_32_R_2
	200_37_F_1	200_40_R_2
	200_44_F_1	200_47_R_2
	200_52_F_3	200_54_R_2
	200_60_F_1	200_64_R_2
	200_71_F_1	200_76_R_2
	200_81_F_1	200_84_R_2
	200_90_F_1	200_92_R_2
	200_98_F_1	200_101_R_2
	200_106_F_3	200_110_R_2
	200_117_F_1	200_122_R_2
	200_126_F_1	200_130_R_2
	200_136_F_1	ZKV_14R_0


